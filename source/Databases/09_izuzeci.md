# Коришћење изузетака

У наставку је приказан пример ускладиштене процедуре `usp_CustomerByCity` у
`Northwind` бази података, која враћа списак купаца из задатог града:

```sql
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE usp_CustomerByCity
    @City NVARCHAR(15)
AS
BEGIN
    SET NOCOUNT ON;
    SELECT CompanyName
    FROM Customers
    WHERE City = @City;
END
GO
```

Креирана је и конзолна апликација која користи ову ускладиштену процедуру и
која је, на основу унетог назива града, генерисала списак купаца који се у том
граду налазе:

```cs
using System;
using System.Data;
using System.Data.SqlClient;

internal class Program
{
    static void Main()
    {
        Console.Write("Unesi naziv grada: ");
        string city = Console.ReadLine();
        string connString = "Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Northwind;Integrated Security=True";
        using (SqlConnection conn = new SqlConnection(connString))
        {
            conn.Open();
            SqlCommand cmd = new SqlCommand("usp_CustomerByCity", conn);
            cmd.CommandType = CommandType.StoredProcedure;
            cmd.Parameters.Add("@City", SqlDbType.NVarChar, 15).Value = city;
            SqlDataAdapter adapter = new SqlDataAdapter(cmd);
            DataTable dt = new DataTable();
            adapter.Fill(dt);
            Console.WriteLine("U zadatom gradu nalaze se sledeći kupci:");
            foreach (DataRow row in dt.Rows)
            {
                Console.WriteLine(row["CompanyName"].ToString());
            }
        }
    }
}
```

Када апликација комуницира са базом података, постоји много потенцијалних
тачака где може доћи до грешке: непостојећи сервер, неисправни подаци, неуспех
у извршавању ускладиштене процедуре, недовољне привилегије, итд. Зато је важно
да се кôд који врши ту комуникацију заштити помоћу обраде изузетака.

```{infonote}
Најчешће грешке које су се дешавале ученицима приликом рада са базом података
су:

* Сервер није доступан или не постоји.
* Неисправан конекциони стринг.
* Грешка приликом дефинисања или постављања вредности параметара у упиту.
* Неслагање типа податка параметра и типа податка поља у бази података.
* Наведен је назив табеле или колоне који не постоји у бази података.
* Упит враћа више редова, а очекује се само један.
* Упит не враћа ниједан ред, иако се очекује да постоје резултати.
* Грешка у самој ускладиштеној процедури.
```

Ово је прави тренутак да поновиш
[градиво о изузецима](https://petlja.org/sr-Latn-RS/kurs/14469/4/9890)
из III разреда. Зашто је важно користити обраду изузетака приликом рада са
базом података? Ако се догоди изузетак, он ће бити ухваћен и апликација неће
прекинути рад на неконтролисан начин. Такође, кориснику можеш приказати
смислену поруку о грешци, на пример: *Није могуће успоставити везу са базом података*.

Грешке се након тога могу логовати како би их касније анализирала особа која је
задужена за одржавање апликације, као што је то чест случај у пракси.

С друге стране, не треба ни претеривати са обрадом изузетака. Оправдано је да
обрадиш изузетке који се тичу грешака у комуникацији са базом података или
неких непредвидивих грешака, али зато није оправдано да се изузецима обрађују
ситуације које се могу унапред спречити самом логиком програма. У претходном
примеру, наредбама услова можеш да провериш да ли је корисник унео назив града
и да ли унети назив града постоји у бази. Модификовани кôд изгледао би овако:

```cs
using System;
using System.Data;
using System.Data.SqlClient;

internal class Program
{
    static void Main()
    {
        Console.Write("Unesi naziv grada: ");
        string city = Console.ReadLine();
        if (string.IsNullOrWhiteSpace(city))
        {
            Console.WriteLine("Obavezan je unos naziva grada.");
            return;
        }
        string connString = "Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Northwind;Integrated Security=True";
        try
        {
            using (SqlConnection conn = new SqlConnection(connString))
            {
                conn.Open();
                SqlCommand cmd = new SqlCommand("usp_CustomerByCity", conn);
                cmd.CommandType = CommandType.StoredProcedure;
                cmd.Parameters.Add("@City", SqlDbType.NVarChar, 15).Value = city;
                SqlDataAdapter adapter = new SqlDataAdapter(cmd);
                DataTable dt = new DataTable();
                adapter.Fill(dt);
                if (dt.Rows.Count == 0)
                {
                    Console.WriteLine("Nema kupaca u zadatom gradu.");
                }
                else
                {
                    Console.WriteLine("U zadatom gradu nalaze se sledeći kupci:");
                    foreach (DataRow row in dt.Rows)
                    {
                        Console.WriteLine(row["CompanyName"].ToString());
                    }
                }
            }
        }
        catch (SqlException ex)
        {
            Console.WriteLine("Greška u komunikaciji sa bazom podataka.");
            Console.WriteLine("Detalji: " + ex.Message);
        }
        catch (Exception ex)
        {
            Console.WriteLine("Došlo je do neočekivane greške.");
            Console.WriteLine("Detalji: " + ex.Message);
        }
    }
}
```

## Класа SqlException

У претходном примеру `try-catch` конструкција има више `catch` блокова у којима
се хватају изузеци. Други изузетак основне `Exception` класе хвата се по
правилу у последњем `catch` блоку, док се у првом хвата изузетак
[`SqlException`](https://learn.microsoft.com/en-us/dotnet/api/system.data.sqlclient.sqlexception?view=netframework-4.8)
класе из именског простора
[`System.Data.SqlClient`](https://learn.microsoft.com/en-us/dotnet/api/system.data.sqlclient?view=netframework-4.8).
Овај изузетак се баца када SQL Server пријави грешку током:

* отварања везе (`SqlConnection.Open()`),
* извршавања команди (`SqlCommand.ExecuteReader()`, `ExecuteNonQuery()`, итд),
* позива ускладиштених процедура,
* слања неисправних параметара,
* проблема са типовима података, индексирањем, дозволама, итд.

Нека од најважнијих својстава ове класе, која ти могу помоћи да што успешније
дебагујеш своју апликацију су:

* `Message` за опис грешке,
* `Number` за SQL Server кôд грешке (нпр. грешка 547 – кршење страних кључева),
* `Server` за име SQL Server инстанце,
* `Procedure` за ускладиштену процедуру у којој је дошло до грешке,
* `LineNumber` за ред у T-SQL скрипти где се десила грешка, и
* `Errors` за колекцију која може да садржи више SQL грешака (тип
`SqlErrorCollection`).

Ако дође до грешака у претходном примеру, први `catch` блок можеш дефинисати на
пример овако:

```cs
catch (SqlException ex)
{
    Console.WriteLine("Greška u komunikaciji sa bazom podataka.");
    Console.WriteLine("Broj greške: " + ex.Number);
    Console.WriteLine("Poruka: " + ex.Message);
    Console.WriteLine("Server: " + ex.Server);
    Console.WriteLine("Procedura: " + ex.Procedure);
    Console.WriteLine("Linija: " + ex.LineNumber);
}
```
