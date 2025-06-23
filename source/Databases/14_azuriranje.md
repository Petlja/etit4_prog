# Ажурирање података у бази

Након што си научио како да читаш и уписујеш податке, следећи логичан корак у
раду са базом је њихово ажурирање. Ово је **Update** операција унутар CRUD
акронима. Процес ажурирања је веома сличан уписивању, али са једном кључном
разликом: мора се прецизно знати који ред у табели треба да се измени. Због
тога је неопходно користити јединствени идентификатор (примарни кључ) тог реда.

За ову лекцију, направићеш апликацију која прво учитава све превознике у
`ComboBox`, омогућава кориснику да изабере једног, прикаже његове тренутне
податке, а затим дозвољава измену и снимање тих података назад у базу.

Као и код уписа, користићеш ускладиштену процедуру да би осигурао безбедност и
ефикасност. Процедура `usp_Izmeni` ће прихватити три параметра:

* `@ShipperID`, јединствени кључ реда који се ажурира,
* `@CompanyName`, нова вредност за назив компаније и
* `@Phone`, нова вредност за телефон.

SQL UPDATE упит користи WHERE клаузулу да би променио само онај ред чији се
`ShipperID` поклапа са прослеђеним параметром.

```sql
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

CREATE PROCEDURE usp_Izmeni
    @ShipperID INT,
    @CompanyName NVARCHAR(40),
    @Phone NVARCHAR(24)
AS
BEGIN
    UPDATE Shippers
    SET 
        CompanyName = @CompanyName,
        Phone = @Phone
    WHERE 
        ShipperID = @ShipperID;
END
GO
```

Можда си приметио да у процедури за ажурирање, за разлику од претходних, нема
наредбе `SET NOCOUNT ON`. Разлог за то је што ова наредба спречава SQL Server
да врати број ажурираних редова. Када је она активна, C# метода
`ExecuteNonQuery()` би вратила -1 уместо стварног броја, што би довело до
погрешног закључка да операција није успела. Уклањањем ове наредбе из
ускладиштене процедуре, осигурава се да ће `ExecuteNonQuery()` вратити тачан
број погођених редова (1 у овом случају), што омогућава апликацији да исправно
верификује успех операције.

Класу `Prevoznik` сада можеш проширити још једном статичком методом
`Azuriraj()`. Ова метода ће бити веома слична методи `Dodaj()`, али ће
прихватати и `ShipperID` као параметар и користиће другу ускладиштену
процедуру. Уместо `ExecuteScalar()`, користиће `ExecuteNonQuery()`. Ова метода
је намењена за извршавање SQL команди које не враћају резултате (као што су
INSERT, UPDATE и DELETE). Она враћа, као што је напоменуто, цео број који
представља број редова на које је упит утицао. За UPDATE упит са WHERE
клаузулом по примарном кључу, очекивана повратна вредност је 1 (ако је ред
пронађен и ажуриран) или 0 (ако ред са тим ID-јем не постоји).

```cs
public static bool Azuriraj(int shipperId, string companyName, string phone)
{
    using (SqlConnection con = new SqlConnection(Konekcija.ConnString))
    using (SqlCommand cmd = con.CreateCommand())
    {
        cmd.CommandText = "usp_Izmeni";
        cmd.CommandType = CommandType.StoredProcedure;
        cmd.Parameters.AddWithValue("@ShipperID", shipperId);
        cmd.Parameters.AddWithValue("@CompanyName", companyName);
        cmd.Parameters.AddWithValue("@Phone", phone);
        con.Open();
        int rowsAffected = cmd.ExecuteNonQuery();
        return rowsAffected == 1;
    }
}
```

Метода `Azuriraj()` враћа `bool` вредност (`true` или `false`) која говори да
ли је операција била успешна. Ово је користан сигнал за презентациони слој.
Презентациони слој за ажурирање је нешто сложенији јер укључује више корака:

* Учитавање података - приликом учитавања форме, мора се попунити `ComboBox`
свим доступним превозницима,
* Избор ставке - када корисник изабере превозника из `ComboBox`-а, морају се
приказати његови тренутни подаци у пољима за унос и
* Чување измена - када корисник кликне на дугме "Izmeni" покреће процес
валидације и позива `Prevoznik.Azuriraj()` методу.

```cs
private void Form1_Load(object sender, EventArgs e)
{
    UcitajPrevoznike();
}

private void UcitajPrevoznike()
{
    try
    {
        List<Prevoznik> prevoznici = Prevoznik.UcitajSve();
        cmbPrevoznici.DisplayMember = "CompanyName";
        cmbPrevoznici.ValueMember = "ShipperID";
        cmbPrevoznici.DataSource = prevoznici;
        cmbPrevoznici.SelectedIndex = -1;
        txtNazivKompanije.Clear();
        txtTelefon.Clear();
    }
    catch (Exception ex)
    {
        MessageBox.Show("Greška prilikom učitavanja: " + ex.Message);
    }
}

private void cmbPrevoznici_SelectedIndexChanged(object sender, EventArgs e)
{
    if (cmbPrevoznici.SelectedItem is Prevoznik izabraniPrevoznik)
    {
        txtNazivKompanije.Text = izabraniPrevoznik.CompanyName;
        txtTelefon.Text = izabraniPrevoznik.Phone;
    }
    else
    {
        txtNazivKompanije.Clear();
        txtTelefon.Clear();
    }
}

private void btnIzmeni_Click(object sender, EventArgs e)
{
    if (cmbPrevoznici.SelectedItem == null)
    {
        MessageBox.Show("Prevoznik mora biti izabran!");
        return;
    }
    if (string.IsNullOrWhiteSpace(txtNazivKompanije.Text) || string.IsNullOrWhiteSpace(txtTelefon.Text))
    {
        MessageBox.Show("Obavezno je popuniti sva polja!");
        return;
    }
    try
    {
        int id = (int)cmbPrevoznici.SelectedValue;
        string naziv = txtNazivKompanije.Text;
        string telefon = txtTelefon.Text;
        bool uspeh = Prevoznik.Azuriraj(id, naziv, telefon);
        if (uspeh)
        {
            MessageBox.Show("Podaci su uspešno ažurirani!");
            UcitajPrevoznike();
        }
        else
        {
            MessageBox.Show("Desila se greška. Ažuriranje nije uspelo!");
            UcitajPrevoznike();
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show("Desila se greška: " + ex.Message);
    }
}
```

Кôд у презентационом слоју сада има јасну динамику: учитај, изабери, прикажи,
измени, сними. Приликом избора ставке у ComboBox-у, користи се догађај
`SelectedIndexChanged`. Након успешног ажурирања, поново се позива метода
`UcitajPrevoznike()` како би ComboBox одражавао најновије стање у бази, што
пружа добру повратну информацију кориснику.

Овим си завршио и са трећом CRUD операцијом, демонстрирајући како се принципи
добре праксе примењују на доследан начин кроз цео животни циклус података у
апликацији.
