# Параметризовани упити и ускладиштене процедуре

Ускладиштене процедуре (енг. *Stored Procedures*) су унапред дефинисане *SQL*
скрипте које се чувају на *SQL Server*-у и могу се позивати из *.NET Framework*
апликација. Оне омогућавају централизацију логике, побољшавају перформансе и
повећавају безбедност.

Зашто је добра идеја да *SQL* упите пишеш у *SQL Server*-у, а не у самој
*.NET Framework* апликацији? Што се перформанси тиче, ако се процедура налази у
*SQL Server*-у, онда ће *SQL Server* бити задужен да компајлира и оптимизује
*SQL* процедуре, што убрзава њихово извршавање. Што се безбедности тиче, због
параметризације упита смањује се ризик од *SQL Injection*-а и омогућава се
контрола приступа. Што се тиче одржавања кода, промене у процедури се врше на
*SQL Server*-у, без потребе за ажурирањем C# кода у апликацији. На крају, једна
ускладиштена процедура у *SQL Server*-у може да се позива из различитих делова
*.NET Framework* апликације.

О ускладиштеним процедурама учио си у III разреду у оквиру наставног предмета
Базе података, али је сада право време да се подсетиш како се креирају
ускладиштене процедуре и како се параметризују упити. Једноставан пример из
претходне лекције захтевао би да се унето корисничко име и лозинка пошаљу
*SQL Server*-у на проверу. Дакле, треба да креираш ускладиштену процедуру која
прима два параметра, `@username` и `@password` и враћа резултат упита.

У SMSS у бази података `Primer` пронађи ставку *Programmability*, па у оквиру
ње пронађи ставку *Stored Procedures*. Десним тастером миша кликни на
*Stored Procedures* па одабери *Stored Procedure...*. Отвориће се шаблон за
креирање упита који креира ускладиштене процедуре, а који изгледа овако:

```sql
-- ================================================
-- Template generated from Template Explorer using:
-- Create Procedure (New Menu).SQL
--
-- Use the Specify Values for Template Parameters 
-- command (Ctrl-Shift-M) to fill in the parameter 
-- values below.
--
-- This block of comments will not be included in
-- the definition of the procedure.
-- ================================================
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:      <Author,,Name>
-- Create date: <Create Date,,>
-- Description: <Description,,>
-- =============================================
CREATE PROCEDURE <Procedure_Name, sysname, ProcedureName> 
    -- Add the parameters for the stored procedure here
    <@Param1, sysname, @p1> <Datatype_For_Param1, , int> = <Default_Value_For_Param1, , 0>, 
    <@Param2, sysname, @p2> <Datatype_For_Param2, , int> = <Default_Value_For_Param2, , 0>
AS
BEGIN
    -- SET NOCOUNT ON added to prevent extra result sets from
    -- interfering with SELECT statements.
    SET NOCOUNT ON;

    -- Insert statements for procedure here
    SELECT <@Param1, sysname, @p1>, <@Param2, sysname, @p2>
END
GO
```

Пракса је да се све ускладиштене процедуре које креира програмер именују са
префиксом `usp_`, јер системске ускладиштене процедуре имају префикс `sp_`.
Твоја ускладиштена процедура ће у овом случају бити прилично једноставна:

```sql
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

CREATE PROCEDURE usp_Login
    @username NVARCHAR(50),
    @password NVARCHAR(50)
AS
BEGIN
    SET NOCOUNT ON;
    SELECT * FROM users WHERE username = @username AND password = @password
END
GO
```

У менију кликни на дугме `Execute` како би се извршио дати упит и креирала
ускладиштена процедура. Када добијеш поруку *Commands completed successfully.*
на тастатури притисни тастер F5, па провери да ли се у секцији
*Programmability / Stored Procedures* налази ускладиштена процедура
`dbo.usp_Login`. Сада можеш да отвориш свој Windows Forms пројекат из претходне
лекције и измениш кôд на следећи начин...

```cs
private void btnLogin_Click(object sender, EventArgs e)
{
    string username = txtUserName.Text;
    string password = txtPassword.Text;
    string connString = "Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Primer;Integrated Security=True";
    using (SqlConnection conn = new SqlConnection(connString))
    {
        conn.Open();
        SqlCommand cmd = new SqlCommand("usp_Login", conn);
        cmd.CommandType = CommandType.StoredProcedure;
        cmd.Parameters.Add("@username", SqlDbType.NVarChar, 50).Value = username;
        cmd.Parameters.Add("@password", SqlDbType.NVarChar, 50).Value = password;
        using (SqlDataReader reader = cmd.ExecuteReader())
        {
            if (reader.HasRows)
            {
                // Pokretanje glavnog dela programa
            }
            else
            {
                MessageBox.Show("Pogresno korisnicko ime ili lozinka!");
            }
        }
    }
}
```

...или, у овом случају уместо метода `Add`, можеш да користиш методе
`AddWithValue`:

```cs
cmd.Parameters.AddWithValue("@username", username);
cmd.Parameters.AddWithValue("@password", password);
```

Шта се променило у коду ове методе у односу на кôд из претходне лекције? Из
кода је нестао *SQL* упит, а у коду се појавила линија која означава да је упит
дефинисан као ускладиштена процедура:

```cs
cmd.CommandType = CommandType.StoredProcedure;
```

Значи, овим се *SQL Server*-у наговештава да уместо *SQL* упита очекује позив
ускладиштене процедуре. Такође, уместо *SQL* упита, у `SqlCommand` наводи се
име процедуре `usp_Login`, а параметри се додају на исти начин као код
параметризованих упита у претходној лекцији. Обрати пажњу да уколико се
параметри ускладиштене процедуре не поклапају по имену или редоследу са оним
што апликација шаље, извршавање може неуспети или дати неочекиване резултате.

Ако желиш да провериш да ли твоја процедура ради без апликације, можеш је
тестирати директно у SSMS-у. Кликом на дугме *New Query* у менију SSMS-а
креирај нови упит, унеси...

```sql
EXEC usp_Login @username = 'paja', @password = 'p@55w0rd'
```

...па кликни на дугме у менију *Execute*. Као резултат добићеш табелу:

| UserID | Username | Password |
|--------|----------|----------|
| 1      | paja     | p@55w0rd |

што значи да је твоја ускладиштена процедура исправна.
