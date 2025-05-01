# Пренос параметара командном објекту

Када се говори о преносу параметара командном објекту, мисли се на начин на
који се вредности прослеђују у SQL упите или ускладиштене процедуре путем
`SqlCommand` објекта. Ово је кључно за: безбедност, јер се избегава могућност
*SQL injection*-а, перформансе јер је боља припрема и кеширање упита, и на
крају, за јасноћу. и одвојеност логике и података.

*SQL injection (SQLi)* је један од најстаријих и најопаснијих облика сајбер
напада, који је први пут документован крајем деведесетих година прошлог века.
Овај напад омогућава хакерима да убаце злонамерне SQL команде у апликације које
користе базе података, чиме могу да приступе осетљивим информацијама, да измене
или униште податке. Компаније могу изгубити милионе због крађе података, као
што је случај са нападом на *Heartland Payment Systems* 2008. године, где је
компромитовано 130 милиона бројева кредитних картица. Поред финансијске штете,
компаније могу трајно оштетити свој имиџ, репутацију и кредибилитет. Можеш само
да замислиш колико страшни могу бити сигурносни пропусти због којих се открију
лични подаци и угрози приватност и безбедност људи. Иако је *SQLi* релативно
лако спречити, он и даље представља велики ризик због своје једноставности и
ефикасности, па је због тога ово изузетно важна лекција!

## Како НЕ ТРЕБА слати упите командном објекту

Замисли да у бази података `Primer` постоји табела `Users` која садржи личне
или осетљиве податке корисника неке апликације. Нека то за пример буду поља
корисничко име (`Username`) и лозинка (`Password`) која је сачувана као
отворени текст, на пример овако:

| UserID | Username | Password |
|--------|----------|----------|
| 1      | paja     | p@55w0rd |
| 2      | raja     | R@j@2025 |
| 3      | gaja     | GajaGaja |
| 4      | vlaja    | Vlaja!!! |
| 5      | zlaja    | ZlAjAaAa |

Програмер је направио форму за пријаву корисника. Ако корисник унесе тачно
корисничко име и лозинку покренуће се главни програм, а у супротном јавиће се
грешка.

*Напомена: У пракси лозинке се никада не чувају у форми отвореног текста - чува
се само њихова хеш вредност!*

Форма за пријаву и кôд догађа клика на дугме за пријаву изгледа овако:

![SQL injection](./images/sqli.png)

```cs
private void btnLogin_Click(object sender, EventArgs e)
{
    string username = txtUsername.Text;
    string password = txtPassword.Text;
    string connString = "Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Primer;Integrated Security=True";
    string sqlUpit = $"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'";
    using (SqlConnection conn = new SqlConnection(connString))
    {
        conn.Open();
        SqlCommand cmd = new SqlCommand(sqlUpit, conn);
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

Када корисник унесе корисничко име и лозинку формира се стринг `sqlUpit`,
отвара се конекција ка бази и упит се прослеђује објекту `SqlCommand` на
извршавање. Ако је корисник унео исправно корисничко име и лозинку, објекат
`reader` имаће један ред па ће се извршити `if` грана и покренути главни део
програма, а ако је унео неисправно корисничко име и/или лозинку извршиће се
`else` грана и јавиће се порука о грешци. На први поглед је све у реду, зар
не?

Ако овакву апликацију користи злонамерни корисник, он у пољу за унос
корисничког имена може унети...

```text
' OR 1 = 1 --
```

...што значи да би формирани стринг `sqlUpit` изгледао овако:

```sql
SELECT * FROM users WHERE username = '' OR 1 = 1 --' AND password = ''
```

Иако корисничко име `''` не постоји у бази података, израз `1 = 1` је тачан,
па ће операција дисјункције `OR` вратити тачно, јер нетачно или тачно даје
тачно. Након израза `1 = 1` кôд је коментарисан помоћу `--`, па неће утицати на
извршавање упита. Како је израз тачан објекат `reader` ће бити попуњен свим
подацима из табеле `Users`, па ће се извршити `if` грана и покренути главни део
програма.

Значи, ако упит садржи било какав унос корисника, никад немој да формираш упит
као стринг! Увек користи параметризоване упите, као у делу лекције који следи.

## Параметризовање упита методом AddWithValue

Да би избегao опасности од *SQL injection* рањивости апликације, потребно је да
користиш параметризоване упите. То значи да SQL упит не садржи директно
кориснички унос као део текста, већ се уместо тога користе параметри који се
додељују преко методе `AddWithValue()`. На тај начин се кориснички унос никада
не тумачи као део SQL синтаксе, већ искључиво као вредност. Ево како можеш исти
пример пријаве корисника написати на безбедан начин:

```cs
private void btnLogin_Click(object sender, EventArgs e)
{
    string username = txtUsername.Text;
    string password = txtPassword.Text;
    string connString = "Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Primer;Integrated Security=True";
    string sqlUpit = "SELECT * FROM users WHERE username = @username AND password = @password";
    using (SqlConnection conn = new SqlConnection(connString))
    {
        conn.Open();
        SqlCommand cmd = new SqlCommand(sqlUpit, conn);
        cmd.Parameters.AddWithValue("@username", username);
        cmd.Parameters.AddWithValue("@password", password);
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

Значи, уместо да у `sqlUpit` директно убацујеш `username` и `password`,
користиш именоване параметре `@username` и `@password`. Методом
`AddWithValue()` се тим параметрима додељују вредности које је корисник унео.
На овај начин, чак и ако неко покуша да убаци SQL кôд (нпр. `' OR 1 = 1 --`),
тај унос ће бити третиран као обична текстуална вредност, а не као део упита.
SQL сервер се брине да ови подаци не угрозе структуру упита, чиме се у
потпуности елиминише ризик од *SQL injection*-а.

Обрати пажњу на именовање параметара - ако користиш `@username` у упиту, мораш
тај исти идентификатор да користиш и у методи `AddWithValue()`. У супротном,
јавиће се грешка јер SQL сервер неће препознати параметар. Назив параметра мора
увек да почне са карактером `@` и у методи `AddWithValue()` и у SQL упиту.
У језику *T-SQL* параметри нису осетљиви на велика/мала слова, али у
програмском језику *C#* јесу, па је зато добра пракса да будеш доследан
приликом именовања идентификатора параметара. О стиловима именовања
идентификатора [учио си у првом разреду](https://petlja.org/sr-Latn-RS/kurs/11231/3/7986).

## Параметризовање упита методом Add

Иако је метода `AddWithValue` згодна за брзо писање кода, у пракси се
препоручује употреба методе `Add`, где се тачно наводи тип података и, ако је
потребно, величина параметра. Ово је посебно важно када радиш са текстуалним
пољима, јер погрешна претпоставка о типу или дужини може довести до неефикасних
упита или грешака у раду са базом.

Претходни пример написан помоћу методе `Add` изгледао би овако:

```cs
private void btnLogin_Click(object sender, EventArgs e)
{
    string username = txtUsername.Text;
    string password = txtPassword.Text;
    string connString = "Data Source=LOCALHOST\\SQLEXPRESS;Initial Catalog=Primer;Integrated Security=True";
    string sqlUpit = "SELECT * FROM users WHERE username = @username AND password = @password";
    using (SqlConnection conn = new SqlConnection(connString))
    {
        conn.Open();
        SqlCommand cmd = new SqlCommand(sqlUpit, conn);
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

Зашто је боље да користиш методу `Add`? Ако је поље `username` у бази типа
`nvarchar(50)`, онда треба и параметар да буде таквог типа. Иначе *SQL Server*
може непотребно да прави конверзију типа или да погрешно оптимизује упит. Када
се тип и величина подударају, *SQL Server* биће ефикаснији приликом извршавања
упита.

Сада је право време да се подсетиш [лекција о типовима података](https://petlja.org/sr-Latn-RS/kurs/14469/2/9846)
у програмском језику C# и упоредиш их са онима које си користио у наставном
предмету Базе података у II и III разреду, односно са типовима података у
*SQL Server Database Engine*-у. *SQL Server* и *.NET Framework* засновани су на
различитим системима типова података. Иако изгледа да имају сличне типове (нпр.
`System.Double` у *.NET Framework*-у и `float` у *SQL Server*-у), њихова
унутрашња структура и прецизност често се разликују. Зато, када апликација чита
или шаље бројеве ка бази, *.NET Framework* алати, попут *SqlDataReader*-а дају
ти могућност да бираш да ли желиш *SQL Server* тип (нпр. `SqlDouble`) или
*.NET Framework* тип (нпр. `double`). Приликом креирања параметара, можеш
прецизно да кажеш који је тип података тако што користиш `DbType.Double` или
`SqlDbType.Float`, како би све било што тачније и како не би дошло до губитка
података.

Комплетна табела за мапирање типова *SQL Server Database Engine*-а и
*.NET Framework*-а можеш погледати у
[прилогу са мапирањем типова](../Appendixes/mapiranje.md).
