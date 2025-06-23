# Уписивање података у базу

У претходним лекцијама научио си како да организујеш кôд у класе и како да
читаш податке из базе података, поштујући принципе трослојне архитектуре. Сада
је време да научиш како се подаци уписују у базу - операција **Create** унутар
CRUD акронима.

За ову лекцију, направићеш једноставну форму која омогућава додавање новог
превозника у табелу `Shippers` у Northwind бази података. Апликација ће имати
поља за унос назива компаније и броја телефона, као и дугме за чување података.

Иако је прилично једноставно написати SQL INSERT упит директно у C# коду, то је
у изузетно лоша пракса, пре свега из безбедносних разлога. Конструисање упита
спајањем стрингова отвара врата за SQL Injection нападе, где злонамерни
корисник може унети SQL кôд у поља за унос и тиме угрозити базу података.
Правилан приступ је коришћење ускладиштених процедура са параметрима. Оне су
сигурније, брже (јер их база података пре-компајлира) и омогућавају бољу
организацију кода.

Ускладиштена процедура `usp_Ubaci` ће прихватити два параметра (`@CompanyName`
и `@Phone`), извршити упис у табелу `Shippers` и вратити `ID` новокреираног
записа помоћу функције SCOPE_IDENTITY().

```sql
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

CREATE PROCEDURE usp_Ubaci
    @CompanyName NVARCHAR(40),
    @Phone NVARCHAR(24)
AS
BEGIN
    SET NOCOUNT ON;

    INSERT INTO Shippers (CompanyName, Phone)
    VALUES (@CompanyName, @Phone);

    SELECT SCOPE_IDENTITY();
END
GO
```

Као и у претходним лекцијама, користићеш `App.config` фајл за чување
конекционог стринга и класу `Konekcija` да би му лако приступио. Њих нема
потребе поново приказивати јер су идентични.

Постојећу класу `Prevoznik` прошири новом статичком методом `Dodaj()`. Ова
метода ће бити део слоја за приступ подацима. Пре него што кренеш са уносом
кода, важно је разумеш шта он треба да ради. Он треба да

* прихвати податке из презентационог слоја (назив компаније и телефон),
* креира `SqlConnection` и `SqlCommand` објекте,
* подеси команду да позива ускладиштену процедуру `usp_Ubaci`,
* "дода" параметре команди на безбедан начин,
* изврши команду и преузме повратну вредност тј. нови ID и
* врати нови ID презентационом слоју као потврду о успеху.

```cs
public static int Dodaj(string companyName, string phone)
{
    using (SqlConnection con = new SqlConnection(Konekcija.ConnString))
    using (SqlCommand cmd = con.CreateCommand())
    {
        cmd.CommandText = "usp_Ubaci";
        cmd.CommandType = CommandType.StoredProcedure;
        cmd.Parameters.AddWithValue("@CompanyName", companyName);
        cmd.Parameters.AddWithValue("@Phone", phone);
        con.Open();
        object result = cmd.ExecuteScalar();
        return Convert.ToInt32(result);
    }
}
```

Кључни део горњег кода је `cmd.Parameters.AddWithValue()`. Ова метода осигурава
да се подаци које је корисник унео третирају као вредности, а не као део SQL
упита. Такође, можеш да приметиш `ExecuteScalar()` методу, која је идеална за
извршавање упита који враћају једну вредност, као што је ID новоуписаног реда.

Када си ово урадио, потребно је да "повежеш" форму са логиком за упис података.
Клик на дугме `btnUnesi` покренуће догађај који ће прикупити податке из
текстуалних поља, извршити основну валидацију и позвати методу
`Prevoznik.Dodaj()`. Као и увек, свака комуникација са базом података мора бити
унутар `try-catch` блока како би могао да обрадиш евентуалне грешке (нпр. ако
база није доступна) и обавестиш корисника на прикладан начин.

```cs
private void btnUnesi_Click(object sender, EventArgs e)
{
    if (string.IsNullOrWhiteSpace(txtNazivKompanije.Text) || string.IsNullOrWhiteSpace(txtTelefon.Text))
    {
        MessageBox.Show("Obavezno je popuniti oba polja!");
        return;
    }
    try
    {
        string naziv = txtNazivKompanije.Text;
        string telefon = txtTelefon.Text;
        int noviId = Prevoznik.Dodaj(naziv, telefon);
        if (noviId > 0)
        {
            MessageBox.Show($"Uspešno je dodat prevoznik ID: {noviId}");
            txtNazivKompanije.Clear();
            txtTelefon.Clear();
            txtNazivKompanije.Focus();
        }
        else
        {
            MessageBox.Show("Desila se greška. Podaci nisu sačuvani.");
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show("Desila se greška: " + ex.Message);
    }
}
```

Логика унутар `btnUnesi_Click` догађаја је јасна: прво се проверава да ли је
корисник унео податке. Ако јесте, позивамо се нашу метода унутар `try-catch`
блока. Уколико је све прошло како треба, добија се ID новог превозника,
обавештава корисника о успеху и припрема форма за следећи унос. У случају
грешке, `catch` блок је хвата и приказује одговарајућу поруку.

На овај начин, успешно си имплементирао **Create** операцију, поштујући принцип
раздвајања одговорности и безбедности.
