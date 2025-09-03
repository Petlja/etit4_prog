# Квиз: генеричке методе

## Питање 1

```{mchoice}
:answer1: Тип променљиве
:answer2: Назив функције
:answer3: Типски параметар
:answer4: Повратна вредност
:correct: 3

Шта означава слово `T` у дефиницији генеричке методе?
```

## Питање 2

```text
public void Metoda<T>(T parametar)
public static <T> void Metoda(T parametar)
public static void Metoda<T>(T parametar)
void Metoda(T parametar)<T>
```

```{mchoice}
:answer1: Правилно је дефинисана метода у првој линији кода.
:answer2: Правилно је дефинисана метода у другој линији кода.
:answer3: Правилно је дефинисана метода у трећој линији кода.
:answer4: Правилно је дефинисана метода у четвртој линији кода.
:correct: 3

Како се правилно дефинише генеричка метода која прихвата један параметар?
```

## Питање 3

```cs
public static void Ispisi<T>(T parametar)
{
    Console.WriteLine(parametar);
}
```

```{mchoice}
:answer1: T
:answer2: 2.2
:answer3: System.Double
:answer4: Ништа. Јавиће се грешка приликом компајлирања.
:correct: 2

Шта ће се исписати након извршавања наредбе: Ispisi(2.2);?
```

## Питање 4

```{mchoice}
:answer1: getType()
:answer2: Type()
:answer3: typeof
:answer4: typeOf()
:correct: 3

Који оператор се користи за добијање типа у току извршавања програма?
```

## Питање 5

```cs
public static void IspisiTip<T>(T parametar)
{
    Console.WriteLine("{0} je tipa {1}", parametar, typeof(T));
}
```

```{mchoice}
:answer1: tri
:answer2: System.String
:answer3: tri je tipa System.String
:answer4: je tipa string
:correct: 3

Шта ће се исписати након позива: IspisiTip("tri");?
```

## Питање 6

```cs
public static void IspisiNiz<T>(T[] niz)
{
    foreach (T element in niz)
    {
        Console.WriteLine(element);
    }
}
```

```{mchoice}
:answer1: Метод враћа низ
:answer2: Метод прима низ променљивих било ког типа
:answer3: Метод прима само низ стрингова
:answer4: Метод не прихвата аргументе
:correct: 2

Шта означава T[] niz у дефиницији дате методе?
```

## Питање 7

```cs
public static void Ispisi<T>(T parametar)
{
    Console.WriteLine(parametar);
}
```

```{mchoice}
:answer1: На основу типа повратне вредности
:answer2: На основу контекста класе
:answer3: На основу типа прослеђеног аргумента
:answer4: Тип се мора увек експлицитно навести
:correct: 3

Како компајлер одређује тип T у позиву Ispisi(1); ако није наведен експлицитно?
```

## Питање 8

```{mchoice}
:answer1: Краћи кôд
:answer2: Боља читљивост
:answer3: Могућност писања општег и поново употребљивог кода
:answer4: Рад са датотекама
:correct: 3

Који је основни разлог за коришћење генеричких метода?
```

## Питање 9

```cs
public static void IspisiNiz<T>(T[] niz)
{
    foreach (T element in niz)
    {
        Console.WriteLine(element);
    }
}

// IspisiNiz(1.1, 2.2, 3.3);
// IspisiNiz<double>(1.1, 2.2, 3.3);
// IspisiNiz(new double[] { 1.1, 2.2, 3.3 });
// IspisiNiz<double>(double[] niz);
```

```{mchoice}
:answer1: Исправан је први позив методе.
:answer2: Исправан је други позив методе.
:answer3: Исправан је трећи позив методе.
:answer4: Исправан је четврти позив методе.
:correct: 3

Како изгледа исправан позив дате методе IspisiNiz за низ реалних бројева?
```

## Питање 10

```cs
public static void IspisiTip<T>(T parametar)
{
    Console.WriteLine("{0} je tipa {1}", parametar, typeof(T));
}
```

```{mchoice}
:answer1: True
:answer2: System.Boolean
:answer3: True je tipa System.Boolean
:answer4: true je tipa bool
:correct: 3

Шта ће се исписати након извршавања IspisiTip(true);?
```
