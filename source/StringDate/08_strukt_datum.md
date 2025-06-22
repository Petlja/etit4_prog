# Структура за рад са датумом и временом

У програмском језику C# структура `DateTime` представља конкретан тренутак у
времену – датум и/или време. Она се најчешће користи за:

* приказ и унос датума и времена,
* одређивање трајања између два датума (уз `TimeSpan`),
* форматирање у различите приказе и
* добијање тренутног датума и времена.

Можеш да креираш фиксни датум и време...

```cs
DateTime datum = new DateTime(2025, 6, 22); // 22. jun 2025.
DateTime datumVreme = new DateTime(2025, 6, 22, 14, 30, 0); // 22. jun 2025. 14:30:00
```

...или тренутни датум и време:

```cs
DateTime sada = DateTime.Now;      // Datum i vreme
DateTime danas = DateTime.Today;   // Samo datum, vreme je 00:00:00
DateTime utc = DateTime.UtcNow;    // Vreme u UTC formatu
```

Основна својства су...

* `Year`, година,
* `Month`, месец (1–12),
* `Day`, дан у месецу,
* `Hour`, сати,
* `Minute`, минути,
* `Second`, секунде,
* `DayOfWeek`, дан у недељи (енумерација `DayOfWeek`) и
* `DayOfYear`, дан у години (1–366).

...и њих можеш користити, на пример овако:

```cs
DateTime d = new DateTime(2025, 12, 31);
Console.WriteLine(d.Year);       // 2025
Console.WriteLine(d.DayOfWeek);  // Wednesday
Console.WriteLine(d.DayOfYear);  // 365
```

И са овом структуром дозвољене су сличне операције као и са `TimeSpan`
структуром. Можеш сабирати и одузимање дане, сате, минуте...

```cs
DateTime danas = DateTime.Today;
DateTime sutra = danas.AddDays(1);
DateTime zaSatVremena = DateTime.Now.AddHours(1);
```

...и израчунати разлику између два датума:

```cs
DateTime pocetak = new DateTime(2025, 1, 1);
DateTime kraj = new DateTime(2025, 6, 22);
TimeSpan trajanje = kraj - pocetak;
Console.WriteLine(trajanje.Days); // 172
```

Метода `ToString()` омогућава приказ датума у различитим форматима, где можеш
да користиш:

* `dd` за дан (две цифре),
* `MM` за месец (две цифре),
* `yyyy` за годину,
* `HH` за сат (00–23) и
* `mm` за минуте.

Подразумевани формат изгледа овако...

```cs
DateTime d = DateTime.Now;
Console.WriteLine(d.ToString()); // npr. 22.6.2025. 14:45:00
```

...а прилагођавањем формата можеш сам да одредиш изглед. На пример:

```cs
DateTime d = new DateTime(2025, 6, 22, 14, 30, 0);
Console.WriteLine(d.ToString("dd.MM.yyyy."));       // 22.06.2025.
Console.WriteLine(d.ToString("HH:mm"));             // 14:30
Console.WriteLine(d.ToString("dd.MM.yyyy. HH:mm")); // 22.06.2025. 14:30
```

Такође, датуме можеш да поредиш:

```cs
DateTime d1 = new DateTime(2025, 1, 1);
DateTime d2 = DateTime.Today;

if (d2 > d1)
{
    Console.WriteLine("Današnji datum je posle 1. januara 2025.");
}
```

Најједноставнији, али небезбедан начин за претварање стринга у датум је методом
`Parse`...

```cs
string unos = "22.06.2025.";
DateTime datum = DateTime.Parse(unos);
```

...јер метода `Parse` може изазвати грешку ако формат није очекиван. Због тога
је боље да користиш `TryParse`:

```cs
string unos = "22.06.2025.";
if (DateTime.TryParse(unos, out DateTime datum))
{
    Console.WriteLine($"Uneti datum je: {datum.ToShortDateString()}");
}
else
{
    Console.WriteLine("Format unetod datuma nije ispravan!");
}
```

Структуре `DateTime` и `TimeSpan` су **вредносни типови**. То значи да се
приликом додељивања копирају по вредности, а не по референци, што их
разликује од класа као што је `string`!

Структура `DateTime` представља основни начин за рад са календарским подацима у
.NET Framework-у. Омогућава приступ појединачним компонентама датума и времена,
поређење, форматирање, као и израчунавање временских разлика. У комбинацији са
`TimeSpan` структуром, пружа све што је потребно за рад са временским подацима.
