# Учитавање података из базе

У претходној лекцији научио си да је издвајање кода у посебне класе једна од
кључних добрих пракси приликом израде .NET Framework апликација које раде са
SQL Server базом података. У лекцијама које следе држаћемо се овог принципа.

Читање података из базе података је једна од четири основне CRUD (CREATE, READ,
UPDATE и DELETE) операције приликом рада са релационом базом података.
Најједноставнији случај може да буде да треба да прикажеш све податке из једне
табеле, на пример, да прикажеш све превознике из табеле `Shippers` из Northwind
базе података у ListView контроли:

![Превозници](./images/Prevoznici1.png)

Како је упит за добијање ових података једноставан и не садржи никакав
кориснички унос, задатак можеш да реализујеш и без ускладиштене процедуре
навођењем упита `SELECT * FROM Shippers` у самој апликацији. Класа за
остваривање конекције на базу података може да изгледа овако...

```cs
using System.Data;
using System.Data.SqlClient;

namespace Prevoznici
{
    internal class Konekcija
    {
        private const string connString = "Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Northwind;Integrated Security=True";
        
        public static SqlCommand GetCommand()
        {
            SqlConnection con = new SqlConnection(connString);
            SqlCommand cmd = new SqlCommand();
            cmd.Connection = con;
            cmd.CommandType = CommandType.Text;
            return cmd;
        }
    }
}
```

...класа `Prevoznik` овако...

```cs
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;

namespace Prevoznici
{
    internal class Prevoznik
    {
        public int ShipperID { get; set; }
        public string CompanyName { get; set; }
        public string Phone { get; set; }

        public static List<Prevoznik> UcitajSve()
        {
            List<Prevoznik> prevoznici = new List<Prevoznik>();
            SqlCommand cmd = Konekcija.GetCommand();
            cmd.CommandText = "SELECT * FROM Shippers";
            SqlDataAdapter da = new SqlDataAdapter(cmd);
            DataTable dt = new DataTable();
            try
            {
                cmd.Connection.Open();
                da.Fill(dt);
                if (dt.Rows.Count > 0)
                {
                    foreach (DataRow dr in dt.Rows)
                    {
                        Prevoznik p = new Prevoznik();
                        p.ShipperID = Convert.ToInt32(dr["ShipperID"]);
                        p.CompanyName = dr["CompanyName"].ToString();
                        p.Phone = dr["Phone"].ToString();
                        prevoznici.Add(p);
                    }
                    return prevoznici;
                }
                else
                {
                    return null;
                    throw new Exception("Tabela je prazna!");
                }
            }
            catch (Exception ex)
            {
                return null;
                throw new Exception("Greska prilikom ucitavanja: " + ex.Message);
            }
            finally
            {
                cmd.Connection.Close();
            }
        }
    }
}
```

...и класа за UI овако:

```cs
using System;
using System.Collections.Generic;
using System.Windows.Forms;

namespace Prevoznici
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            PrevozniciListView.View = View.Details;
            PrevozniciListView.Columns.Add("ID", 10);
            PrevozniciListView.Columns.Add("Kompanija", 40);
            PrevozniciListView.Columns.Add("Telefon", 24);
            PrevozniciListView.FullRowSelect = true;
            List<Prevoznik> prevoznici = Prevoznik.UcitajSve();
            string[] polje = new string[3];
            foreach (Prevoznik p in prevoznici)
            {
                polje[0] = p.ShipperID.ToString();
                polje[1] = p.CompanyName;
                polje[2] = p.Phone;
                ListViewItem unos = new ListViewItem(polje);
                PrevozniciListView.Items.Add(unos);
                PrevozniciListView.AutoResizeColumns(ColumnHeaderAutoResizeStyle.ColumnContent);
            }
        }
    }
}
```

За разлику од контроле `ListView` која омогућава приказ у више колона, контрола
`ListBox` приказује само једну вредност за сваки објекат. Ако треба да прикажеш
резултате у контроли `ListBox`...

![Превозници](./images/Prevoznici2.png)

...онда је најједноставније да надјачаш методу `ToString()` у класи `Prevoznik`
овако...

```cs
public override string ToString()
{
    return String.Format("{0,-3}{1,-18}{2,-16}", ShipperID, CompanyName, Phone);
}
```

...а притом обратиш пажњу на највећу могућу ширину података у формату стринга.
Након тога можеш да, у класи за UI, употребиш `ListBox` уместо `ListView`...

```cs
private void Form1_Load(object sender, EventArgs e)
{
    List<Prevoznik> prevoznici = Prevoznik.UcitajSve();
    PrevozniciListBox.Font = new Font("Consolas", 10);
    PrevozniciListBox.DataSource = prevoznici;
    PrevozniciListBox.SelectedIndex = -1;
}
```

...где треба да обратиш пажњу да је неопходно да користиш *моноширинске* (енгл.
*monospaced* или *fixed-width*) фонтове попут `Terminal`, `Courier New`,
`Consolas` итд.

Ако је потребно да се резултати прикажу у контроли `ComboBox`, на пример,
вредности из колоне `CompanyName`...

![Превозници](./images/Prevoznici3.png)

...надјачана метода `ToString()` у класи `Prevoznik` могла би да изгледа
овако...

```cs
public override string ToString()
{
    return CompanyName;
}
```

...а кôд у класи за UI овако:

```cs
private void Form1_Load(object sender, EventArgs e)
{
    List<Prevoznik> prevoznici = Prevoznik.UcitajSve();
    PrevozniciComboBox.DataSource = prevoznici;
    PrevozniciComboBox.SelectedIndex = -1;
}
```

Учитавање података из базе података и приказ у контролама `DataGridView` и
`Chart`, биће тема у некој од наредних лекција.
