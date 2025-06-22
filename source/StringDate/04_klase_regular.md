# Класе именског простора RegularExpressions

Именски простор
[`RegularExpressions`](https://learn.microsoft.com/en-us/dotnet/api/system.text.regularexpressions?view=netframework-4.8.1)
обезбеђује класе за рад са регуларним изразима. Основна класа коју ћеш најчешће
користити је [`Regex`](https://learn.microsoft.com/en-us/dotnet/api/system.text.regularexpressions.regex?view=netframework-4.8.1)
али постоји и више других помоћних класа, енумерација и делегата који
омогућавају напредну обраду података. Неки од њих су:

* `Match` је класа која представља резултат једног поклапања регуларног израза
са стрингом,
* `MatchCollection` представља скуп свих поклапања у једном стрингу,
* `Group` представља део поклапања дефинисан заградама у регуларном изразу,
* `GroupCollection` представља скуп свих група у оквиру једног поклапања,
* `Capture` представља конкретан део текста који је поклопљен,
* `CaptureCollection` представља све појаве поклапања за један елемент и
* `RegexOptions` је енумерација која одређује додатне опције за рад са
регуларним изразима.

## Провера поклапања

Метода `IsMatch()` враћа `True` уколико је наведени регуларни израз проналашао
поклапање у наведеном улазном стрингу, односно `False` уколико није. На пример:

```cs
string tekst = "Moj broj telefona je 063/123-4567";
string sablon = @"\d{3}/\d{3}-\d{4}";

bool sadrziBroj = Regex.IsMatch(tekst, sablon);
Console.WriteLine($"Pronadjeno: {sadrziBroj}"); // Pronadjeno: True
```

## Проналажење првог поклапања

Метода `Match()` претражује прво појављивање регуларног израза у наведеном
улазном стрингу. На пример:

```cs
string tekst = "Danas je 28.05.2025. godine.";
string sablon = @"\d{2}\.\d{2}\.\d{4}";
Match m = Regex.Match(tekst, sablon);
if (m.Success)
{
    Console.WriteLine($"Pronadjen datum: {m.Value}"); // Pronadjen datum: 28.05.2025.
    Console.WriteLine($"Pozicija: {m.Index}");        // Pozicija: 9
}
```

Ако исти шаблон треба да користиш више пута, можеш инстанцирати `Regex`
објекат, на пример:

```cs
Regex reg = new Regex(@"\d{4}");
bool postoji = reg.IsMatch("Godina je 2025");
Match m = reg.Match("Godina: 2025");
```

## Проналажење свих поклапања

Метода `Matches()` претражује наведени улазни стринг за сва појављивања
наведеног регуларног израза. На пример:

```cs
string tekst = "Kontakt telefoni: 062-123-456, 063-789-012, 064-555-666";
string sablon = @"\d{3}-\d{3}-\d{3}";
MatchCollection mc = Regex.Matches(tekst, sablon);
Console.WriteLine($"Ukupno pronadjeno {mc.Count} broja:");  // Ukupno pronadjeno 3 broja:
foreach (Match m in mc)
{                                                           // * 062-123-456
    Console.WriteLine($"* {m.Value}");                      // * 063-789-012
}                                                           // * 064-555-666
```

## Замена текста

Метода `Replace()`, у наведеном улазном низу, замењује све низове који се
подударају са наведеним регуларним изразом са наведеним низом за замену.

```cs
string tekst = "Moja e-mail adresa je petar@petrovic.com";
string sablon = @"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b";
string zamena = "[SAKRIVEN EMAIL]";
string rezultat = Regex.Replace(tekst, sablon, zamena);
Console.WriteLine(rezultat); // Moja e-mail adresa je [SAKRIVEN EMAIL]
```

## Групе поклапања

У шаблону можеш користити заграде `()` за дефинисање група (подделова), чиме се
добија колекција `Groups`:

* `Groups[0]` за цело поклапање
* `Groups[1]`, `Groups[2]`... за појединачне групе

```cs
string tekst = "Ime: Velimir, godina: 18";
Match m = Regex.Match(tekst, @"Ime: (\w+), godina: (\d+)");

string ime = m.Groups[1].Value;    // Velimir
string godina = m.Groups[2].Value; // 18
```

Када је једна група дефинисана као понављајућа, могуће је добити више ухваћених
појава те групе преко својства `Captures`, на пример:

```cs
string tekst = "1,2,3";
Match m = Regex.Match(tekst, @"(\d,?)+");

foreach (Capture c in m.Groups[1].Captures)
{
    Console.WriteLine(c.Value); // 1, 2, 3
}
```

## Контрола понашања Regex објекта

За контролу понашања `Regex` објекта можеш да користиш енумерацију
`RegexOptions`:

* `IgnoreCase` игнорише разлику између великих и малих слова,
* `Multiline` да се `^` и `$` односе на почетак и крај сваког реда,
* `Singleline` омогућава да `.` обухвати и знак новог реда,
* `Compiled` компајлира израз за брже извршавање и
* `IgnorePatternWhitespace` игнорише размаке у шаблону.

На пример:

```cs
Regex regex = new Regex(@"^ime:\s+\w+$", RegexOptions.IgnoreCase);
bool validno = regex.IsMatch("Ime: Jovan"); // true
```

Рад са регуларним изразима у .NET Framework-у се ослања на класе именског
простора `RegularExpressions`. Ове класе омогућавају једноставну примену
шаблона, као и напредно руковање групама и поклапањима. Разумевање класа као
што су `Regex`, `Match`, `Group` и `Capture` омогућава креирање робусних и
флексибилних решења за обраду текста.

У наредној лекцији биће обрађена контрола уноса текстуалних података, односно
како у оквиру графичког корисничког интерфејса омогућити валидацију уноса у
реалном времену.
