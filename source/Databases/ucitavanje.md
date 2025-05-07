# Учитавање података из базе

У претходним лекцијама већ си учитавао податке из Northwind базе података.
Наглашено је да током рада апликација може да приступа бази података на два
начина: конекционо и бесконекционо. У лекцијама које следе, фокус ће углавном
бити на бесконекционом приступу и раду са објектима типа `DataTable`.

У претходној лекцији научио си да је издвајање кода у посебне класе једна од
кључних добрих пракси при изради .NET Framework апликација које раде са SQL
Server базом података, као и да кôд који се односи на кориснички интерфејс,
логику апликације и приступ бази података треба да буде јасно раздвојен.

Оно што је заједничко свим апликацијама које приступају бази података јесте да
прво морају да се повежу на базу података. Тако, кôд служи само за повезивање
на базу података можеш да издвојиш у посебну класу, нпр. класу `Konekcija.cs`.
На овај начин остварујеш јасно одвајање одговорности (јер класа `Konekcija`
служи само за повезивање), лакоћу употребе (јер друге класе лако долазе до
повезаног SqlCommand објекта) и једне тачке за промену конекционог стринга (јер
ће бити довољно да се промени само на једном месту). Класа `Konekcija.cs` може
да изгледа овако:

```cs
using System.Data.SqlClient;

public static class Konekcija
{
    public static SqlCommand GetOpenCommand()
    {
        SqlConnection con = new SqlConnection("Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Northwind;Integrated Security=True");
        con.Open();
        SqlCommand cmd = new SqlCommand();
        cmd.Connection = con;
        return cmd;
    }
}
```

На овај начин класа `Konekcija` обезбеђује `SqlCommand` објекат са отвореном
везом. Сада се из других класа, на пример класе `Program` у конзолној
апликацији може користити `SqlDataAdapter` и `DataTable` за учитавање података
из базе, на пример:

```cs
using System;
using System.Data;
using System.Data.SqlClient;

class Program
{
    static void Main()
    {
        DataTable dt = new DataTable();
        using (SqlCommand cmd = Konekcija.GetOpenCommand())
        {
            cmd.CommandText = "SELECT ProductName FROM Products";
            SqlDataAdapter da = new SqlDataAdapter(cmd);
            da.Fill(dt);
        }
        foreach (DataRow dr in dt.Rows)
        {
            Console.WriteLine(dr["ProductName"].ToString());
        }
    }
}
```
