# Рад са класама

Сада је прави тренутак да се подсетиш основа објектно-оријентисаног
програмирања,
[рада са класама](https://petlja.org/sr-Latn-RS/kurs/14469/3/9869)
и
[изведеним класама](https://petlja.org/sr-Latn-RS/kurs/14469/5/9900). Издвајање
кода у посебне класе је једна од кључних добрих пракси при изради .NET
Framework апликација које раде са SQL Server базом података. Овај приступ
доноси бројне предности у погледу организације, одржавања, тестирања и поновне
употребе кода.

Што се раздвајање одговорности (енгл. *Separation of Concerns*) тиче, кôд који
се односи на кориснички интерфејс, логику апликације и приступ бази података
треба да буде јасно раздвојен. То олакшава одржавање и разумевање кода. Поновна
употреба кода подразумева да једном написан кôд за приступ бази може да се
користи у више делова апликације. Такође, издвојене класе се лакше тестирају
јер су изоловане, а цео пројекат постаје прегледнији и лакше га је проширивати.

Најчешће се програмери, приликом израде оваквих апликација, воде трослојном
архитектуром (енгл. *Three-Layer Architecture*). Први слој за приступ подацима
(енгл. *Data Access Layer - DAL*) треба да садржи сав кôд којим се директно
приступа бази и у њему се обично користе објекти класа `SqlConnection`,
`SqlCommand` и `SqlDataAdapter`. Други слој логике пословања (енгл.
*Business Logic Layer - BLL*) бави се обрадом правила пословања и позива методе
из слоја за приступ подацима. Трећи презентациони слој (енгл.
*Presentation Layer - UI*) одговоран је за комуникацију са корисником и позива
методе из слоја логике пословања. Ако је у питању мала апликација, други слој
ће се често утопити са првим слојем.

На пример, нека је у питању једноставна Windows Forms апликација за преглед
цена производа из табеле `Products` у бази података Northwind.

![Цене свих производа](./images/CeneSvihProizvoda.png)

На основу правила трослојне архитектуре апликација могла би да поседује више
класа:

* Класа за остваривање конекције на базу података Northwind, (нпр. класа
`Konekcija`) треба да садржи само кôд који служи за повезивање на базу. На овај
начин остварујеш јасно одвајање одговорности (јер класа `Konekcija` служи само
за повезивање), лакоћу употребе (јер друге класе лако долазе до `SqlCommand`
објекта) и једне тачке за промену конекционог стринга (јер ће бити довољно да
се конекциони стринг мења само на овом месту).
* Класа података о производу (нпр. класа `Proizvod`) треба да имплементира
комуникацију са базом података извршавајући ускладиштене процедуре, односно SQL
упите. У овој класи би се креирали и користили објекти `SqlConnection`,
`SqlCommand` и `SqlDataAdapter` и мапирали подаци у објекте.
* Класа за обраду података треба да имплементира правила пословања и обради
податаке пре и после приступа бази. Она може да садржи пословну логику као што
је филтрирање, валидација или трансформација података. Помоћу ње се може и
одлучивати када и како треба позивати методе из класе за приступ подацима.
Пошто се у овом случају ради о изузетно једноставној апликацији где се подаци
само приказују, нема потребе креирати ову класу.
* Класа за презентацију података (нпр. класа `CeneProizvodaForm`), треба да
имплементира контроле за приказ података, контроле за прикупљање података од
корисника, контроле за позивање метода из класе за обраду података итд.

Пре него што почнеш са креирањем класа, потребно је да напишеш ускладиштену
процедуру која ће вратити тражене податке:

```sql
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

CREATE PROCEDURE usp_Proizvodi
AS
BEGIN
    SET NOCOUNT ON;
    SELECT ProductID, ProductName, UnitPrice FROM Products
END
GO
```

Класа за остваривање конекције на базу података може да изгледа овако...

```cs
using System.Data;
using System.Data.SqlClient;

namespace CeneSvihProizvoda
{
    internal class Konekcija
    {
        private const string connString = "Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Northwind;Integrated Security=True";
        
        public static SqlCommand GetCommand()
        {
            SqlConnection con = new SqlConnection(connString);
            SqlCommand cmd = new SqlCommand();
            cmd.Connection = con;
            cmd.CommandType = CommandType.StoredProcedure;
            return cmd;
        }
    }
}
```

...класа података овако...

```cs
using System;
using System.Data;
using System.Data.SqlClient;

namespace CeneSvihProizvoda
{
    internal class Proizvod
    {
        public static DataTable Prikazi()
        {
            SqlCommand cmd = Konekcija.GetCommand();
            cmd.CommandText = "usp_Proizvodi";
            SqlDataAdapter da = new SqlDataAdapter(cmd);
            DataTable dt = new DataTable();
            try
            {
                cmd.Connection.Open();
                da.Fill(dt);
                if (dt.Rows.Count > 0)
                {
                    return dt;
                }
                else
                {
                    throw new Exception("Tabela je prazna!");
                }
            }
            catch (Exception ex)
            {
                throw;
            }
            finally
            {
                cmd.Connection.Close();
            }
        }
    }
}
```

и на крају, класа за презентацију података овако:

```cs
using System;
using System.Data;
using System.Windows.Forms;

namespace CeneSvihProizvoda
{
    public partial class CeneProizvodaForm : Form
    {
        public CeneProizvodaForm()
        {
            InitializeComponent();
        }

        private void PrikazBtn_Click(object sender, EventArgs e)
        {
            try
            {
                DataTable dt = Proizvod.Prikazi();
                CeneProizvodaGrid.DataSource = dt;
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message);
            }
        }

        private void IzlazBtn_Click(object sender, EventArgs e)
        {
            Application.Exit();
        }
    }
}
```
