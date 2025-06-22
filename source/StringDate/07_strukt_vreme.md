# Структура за рад са временом

У програмском језику C# рад са временом (сатима, минутима, секундама) реализује
се помоћу структуре `TimeSpan` из именског простора `System`. `TimeSpan`
представља интервал времена, односно разлику између два временска тренутка
(нпр. "5 дана", "2 сата и 30 минута", "45 секунди" итд).

Временски интервал можеш креирати помоћу конструктора...

```cs
TimeSpan trajanje = new TimeSpan(1, 20, 30); // 1 sata, 20 minuta i 30 sekundi
```

...или помоћу статичких метода:

```cs
TimeSpan t1 = TimeSpan.FromHours(1.5);     // 1 sat i 30 minuta
TimeSpan t2 = TimeSpan.FromMinutes(45);    // 45 minuta
TimeSpan t3 = TimeSpan.FromSeconds(10);    // 10 sekundi
```

Основна својства су:

* `Hours`, број сати,
* `Minutes`, број минута,
* `Seconds`, број секунди,
* `Days`, број дана,
* `TotalHours`, укупно сати као децимални број,
* `TotalMinutes`, укупно минута као децимални број и
* `TotalSeconds`, укупно секунди као децимални број.

Пошто са временом радиш као са типом података, дозвољене су одређене операције.
Сабирање и одузимање временских интервала врши се једноставно, операторима `+`
и `-`. На пример:

```cs
TimeSpan t1 = TimeSpan.FromMinutes(30);
TimeSpan t2 = TimeSpan.FromMinutes(45);

TimeSpan zbir = t1 + t2;        // 1:15
TimeSpan razlika = t2 - t1;     // 0:15

Console.WriteLine(zbir);        // 01:15:00
Console.WriteLine(razlika);     // 00:15:00
```

Дозвољено је и поређење релацијским операторима. На пример:

```cs
if (t1 < t2)
{
    Console.WriteLine("t1 je manji interval od t2");
}
```

`TimeSpan` може представљати и негативну вредност:

```cs
TimeSpan ts = TimeSpan.FromMinutes(-10);
Console.WriteLine(ts); // -00:10:00
```

`TimeSpan` се често користи у комбинацији са структуром `DateTime` ради
израчунавања разлике између два временска тренутка. На пример:

```cs
DateTime pocetak = DateTime.Now;
DateTime kraj = pocetak.AddMinutes(90);

TimeSpan trajanje = kraj - pocetak;
Console.WriteLine($"Trajanje: {trajanje.TotalMinutes} minuta");   // 90
```

За прилагођени формат резултата мораш да користиш прелазне секвенце (`\:`). На
пример:

```cs
TimeSpan ts = new TimeSpan(2, 5, 30);
Console.WriteLine(ts.ToString());           // 02:05:30
Console.WriteLine(ts.ToString(@"hh\:mm"));  // 02:05
```

Структура `TimeSpan` представља основни начин за рад са интервалима времена у
програмском језику C#. Она омогућава креирање, сабирање, одузимање и поређење
временских трајања, као и прецизну контролу над радом са сатима, минутима и
секундама. У наредној лекцији научићеш да користиш структуру `DateTime`, која
представља конкретан датум и време.
