# Квиз: генеричке методе са више параметара

## Питање 1

```cs
public static void Zameni<T>(ref T a, ref T b)
{
    T temp = a;
    a = b;
    b = temp;
}
```

```{mchoice}
:answer1: Метод прима два параметра која су типа string
:answer2: Метод прима два параметра различитих типова
:answer3: Метод прима два параметра истог типа, одређеног током позива
:answer4: Метод не може примити параметре по референци
:correct: 3

Шта значи запис `Zameni<T>(ref T a, ref T b)` у дефиницији методе?
```

## Питање 2

```{mchoice}
:answer1: Да би се проследили параметри по вредности
:answer2: Да би се омогућила измена вредности аргумената у позивајућем коду
:answer3: Да би се избегле грешке у компајлирању
:answer4: Да би се позвала метода више пута
:correct: 2

Зашто се у методи `Zameni<T>` у претходном питању користи кључна реч `ref`?
```

## Питање 3

```cs
int a = 1, b = 2;
Zameni(ref a, ref b);
Console.WriteLine("{0} {1}", a, b);
```

```{mchoice}
:answer1: 1 2
:answer2: 2 1
:answer3: 0 0
:answer4: Ништа. Јавиће се грешка приликом компајлирања.
:correct: 2

Шта ће се исписати након извршења датог кода, ако се позива метода из првог
питања?
```

## Питање 4

```text
public static void Metoda<T>(T1 a, T2 b, T3 c)
public static void Metoda<T1, T2, T3>(T1 a, T2 b, T3 c)
public static void Metoda(a, b, c)
public void Metoda<T>(T a, T b, T c)
```

```{mchoice}
:answer1: Правилно је дефинисана метода у првој линији кода.
:answer2: Правилно је дефинисана метода у другој линији кода.
:answer3: Правилно је дефинисана метода у трећој линији кода.
:answer4: Правилно је дефинисана метода у четвртој линији кода.
:correct: 2

Како изгледа исправна дефиниција методе која прима параметре различитих типова?
```

## Питање 5

```cs
public static void IspisiVrednosti<T1, T2, T3>(T1 v1, T2 v2, T3 v3)
{
    Console.WriteLine("{0}, {1}, {2}", v1, v2, v3);
}
```

```{mchoice}
:answer1: 1 2.3 3.14
:answer2: 1, 2.3, 3.14159265358979
:answer3: System.Int32, System.Double, System.Double
:answer4: 1 2.3 PI
:correct: 2

Шта ће се исписати након позива IspisiVrednosti(1, 2.3, Math.PI);
```

## Питање 6

```{mchoice}
:answer1: Типска конверзија
:answer2: Типско наслеђивање
:answer3: Инференција типова
:answer4: Генеричко проширење
:correct: 3

Како се зове појава када компајлер сâм закључи типске параметре на основу
аргумената?
```

## Питање 7

```cs
public static string SpojiTriVrednosti<T1, T2, T3>(T1 v1, T2 v2, T3 v3)
{
    return $"{v1} {v2} {v3}";
}
```

```{mchoice}
:answer1: "tri 2.2 1"
:answer2: "1, 2.2, tri"
:answer3: "1 2.2 tri"
:answer4: System.String
:correct: 3

Шта враћа `SpojiTriVrednosti(1, 2.2, "tri")`?
```

## Питање 8

```text
public static void Metoda<T>(T[] niz)
public static string SpojiVrednosti(params object[] parametri)
public static void Metoda<T1, T2>(params T1[] n)
public static object[] Spoji(params string[] vrednosti)
```

```{mchoice}
:answer1: Правилно је дефинисана метода у првој линији кода.
:answer2: Правилно је дефинисана метода у другој линији кода.
:answer3: Правилно је дефинисана метода у трећој линији кода.
:answer4: Правилно је дефинисана метода у четвртој линији кода.
:correct: 2

Која је правилно дефинисана метода која прима променљив број параметара било
ког типа?
```

## Питање 9

```cs
public static string SpojiVrednosti(params object[] parametri)
{
    return string.Join(" ", parametri);
}
```

```{mchoice}
:answer1: "1 2.2 tri C Pozdrav!"
:answer2: 1, 2.2, tri, C, Pozdrav!
:answer3: Spojeno
:answer4: System.Object[]
:correct: 1

Шта ће бити резултат позива `SpojiVrednosti(1, 2.2, "tri", 'C', "Pozdrav!")`
```

## Питање 10

```{mchoice}
:answer1: Јер ради само са стринговима
:answer2: Јер има фиксно дефинисан број параметара
:answer3: Јер подржава било који број параметара било ког типа
:answer4: Јер користи ref параметре
:correct: 3

Зашто је метода `SpojiVrednosti` из деветог питања флексибилнија од методе
`SpojiTriVrednosti` из седмог питања?
```
