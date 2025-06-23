# Брисање података из базе

Стигао си до последње од четири основне CRUD операције: брисање података тј.
операиције **Delete**. Као и код ажурирања, операција брисања захтева прецизно
дефинисање одређеног реда у табели помоћу његовог јединственог идентификатора
(примарног кључа). Поступак је веома опасан ако се не изведе пажљиво, јер
једном обрисани подаци се, у већини случајева, не могу вратити.

Треба да имаш у виду да у комплексним базама података, често нећеш моћи да
обришеш ред ако други редови у другим табелама зависе од њега (нпр. не можеш
обрисати превозника ако постоје поруџбине које су повезане са њим). Ово се зове
референцијални интегритет и штити базу од неконзистентних података. У том
случају, DELETE наредба би изазвала грешку, коју би апликација требало да
ухвати и обради. За потребе ове лекције, претпостави да таква ограничења не
постоје.

У овој лекцији, направићеш апликацију која, слично претходној, учитава
превознике у ComboBox и омогућава кориснику да изабере једног и обрише га из
базе података, уз обавезну потврду пре саме операције.

Безбедност је и овде на првом месту. Креираћеш ускладиштену процедуру
`usp_Obrisi` која ће прихватити само један параметар: `@ShipperID`, тј. ID реда
који треба да буде обрисан. DELETE наредба ће користити WHERE клаузулу како би
осигурала да се обрише само и тачно тај ред.

```sql
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

CREATE PROCEDURE usp_Obrisi
    @ShipperID INT
AS
BEGIN
    DELETE FROM Shippers
    WHERE ShipperID = @ShipperID;
END
GO
```

Као и код UPDATE процедуре, и овде je намерно изостављен `SET NOCOUNT ON` како
би C# апликација могла да добије тачан број обрисаних редова.

У класу `Prevoznik` можеш сада да напишеш и последњу CRUD методу: `Obrisi()`.
Ова метода ће бити веома слична методи `Azuriraj()`. Прихватиће `shipperId` као
параметар, позваће `ExecuteNonQuery()` и вратиће `bool` вредност која означава
да ли је брисање било успешно.

```cs
public static bool Obrisi(int shipperId)
{
    using (SqlConnection con = new SqlConnection(Konekcija.ConnString))
    using (SqlCommand cmd = con.CreateCommand())
    {
        cmd.CommandText = "usp_Obrisi";
        cmd.CommandType = CommandType.StoredProcedure;
        cmd.Parameters.AddWithValue("@ShipperID", shipperId);
        con.Open();
        int rowsAffected = cmd.ExecuteNonQuery();
        return rowsAffected > 0;
    }
}
```

Логика је идентична као код ажурирања: позива се процедура и проверава да ли је
број погођених редова већи од нуле.

Презентациони слој за брисање је једноставан, али мора да укључи један веома
важан елемент корисничког искуства: дијалог за потврду. Никада не би требало
дозволити брисање података једним кликом. Увек треба питати корисника да ли је
сигуран.

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

private void btnObrisi_Click(object sender, EventArgs e)
{
    if (cmbPrevoznici.SelectedItem == null)
    {
        MessageBox.Show("Prevoznik mora biti izabran!");
        return;
    }
    string poruka = $"Da li ste sigurni da želite da obrišete prevoznika: '{cmbPrevoznici.Text}'?";
    DialogResult rezultat = MessageBox.Show(poruka, "Potvrda brisanja", MessageBoxButtons.YesNo, MessageBoxIcon.Warning);
    if (rezultat != DialogResult.Yes)
    {
        return;
    }
    try
    {
        int idZaBrisanje = (int)cmbPrevoznici.SelectedValue;
        bool uspeh = Prevoznik.Obrisi(idZaBrisanje);
        if (uspeh)
        {
            MessageBox.Show("Prevoznik je uspešno obrisan");
            UcitajPrevoznike();
        }
        else
        {
            MessageBox.Show("Desila se greška. Brisanje nije uspelo!");
        }
    }
    catch (Exception ex)
    {
        if (ex.Message.Contains("REFERENCE constraint"))
        {
            MessageBox.Show("Brisanje nije moguće zbog referencijalnog integriteta!");
        }
        else
        {
            MessageBox.Show("Desila se greška: " + ex.Message);
        }
    }
}
```
