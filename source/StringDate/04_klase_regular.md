# Класе именског простора RegularExpressions

Именски простор
[`RegularExpressions`](https://learn.microsoft.com/en-us/dotnet/api/system.text.regularexpressions?view=netframework-4.8.1)
обезбеђује класе за рад са регуларним изразима. Основна класа коју ћеш најчешће
користити је `Regex` али постоји и више других помоћних класа, енумерација и
делегата који омогућавају напредну обраду података. Неки од њих су:

* `Match` је класа која представља резултат једног поклапања регуларног израза
са стрингом,
* `MatchCollection` представља скуп свих поклапања у једном стрингу,
* `Group` представља део поклапања дефинисан заградама у регуларном изразу,
* `GroupCollection` представља скуп свих група у оквиру једног поклапања,
* `Capture` представља конкретан део текста који је поклопљен,
* `CaptureCollection` представља све појаве поклапања за један елемент и
* `RegexOptions` је енумерација која одређује додатне опције за рад са
регуларним изразима.

Класа коју ћеш најчешће користити приликом рада са регуларним изразима је класа
[`Regex`](https://learn.microsoft.com/en-us/dotnet/api/system.text.regularexpressions.regex?view=netframework-4.8.1).

## Провера поклапања

Метода `IsMatch()` враћа `True` уколико је наведени регуларни израз проналашао
поклапање у наведеном улазном стрингу, односно `False` уколико није. На пример:

```cs
string text = "Moj broj telefona je 063/123-4567";
string pattern = @"\d{3}/\d{3}-\d{4}";

bool hasPhoneNumber = Regex.IsMatch(text, pattern);
Console.WriteLine($"Pronadjeno: {hasPhoneNumber}"); // Pronadjeno: True
```

## Проналажење првог поклапања

Метода `Match()` претражује прво појављивање регуларног израза у наведеном
улазном стрингу. На пример:

```csharp
string text = "Danas je 28.05.2025. godine.";
string pattern = @"\d{2}\.\d{2}\.\d{4}";
Match match = Regex.Match(text, pattern);
if (match.Success)
{
    Console.WriteLine($"Pronadjen datum: {match.Value}"); // Pronadjen datum: 28.05.2025.
    Console.WriteLine($"Pozicija: {match.Index}");        // Pozicija: 9
}
```

## Проналажење свих поклапања

Метода `Matches()` претражује наведени улазни стринг за сва појављивања
наведеног регуларног израза. На пример:

```csharp
string text = "Kontakt telefoni: 062-123-456, 063-789-012, 064-555-666";
string pattern = @"\d{3}-\d{3}-\d{3}";
MatchCollection matches = Regex.Matches(text, pattern);
Console.WriteLine($"Ukupno pronadjeno {matches.Count} broja:");
foreach (Match match in matches)                // Ukupno pronadjeno 3 broja:
{                                               // * 062-123-456
    Console.WriteLine($"* {match.Value}");      // * 063-789-012
}                                               // * 064-555-666
```

## Замена текста

Метода `Replace()`, у наведеном улазном низу, замењује све низове који се
подударају са наведеним регуларним изразом са наведеним низом за замену.

```csharp
string text = "Moja e-mail adresa je petar@petrovic.com";
string pattern = @"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b";
string replacement = "[SAKRIVEN EMAIL]";
string result = Regex.Replace(text, pattern, replacement);
Console.WriteLine(result); // Moja e-mail adresa je [SAKRIVEN EMAIL]
```
