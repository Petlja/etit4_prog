# Квиз: ограничења за типске параметре

## Питање 1

```{mchoice}
:answer1: where T : class
:answer2: where T : struct
:answer3: where T : value
:answer4: where T == int
:correct: 2

Која је исправна синтакса за постављање ограничења да типски параметар мора
бити вредносни тип?
```

## Питање 2

```cs
public static void Podrazumevano<T>() where T : struct
{
    Console.WriteLine(default(T));
}
```

```{mchoice}
:answer1: Метода ће се извршити и исписати null
:answer2: Биће приказано име типа String
:answer3: Десиће се грешка приликом компајлирања
:answer4: Извршиће се, али ништа неће бити исписано
:correct: 3

Шта ће се десити ако позовеш `Podrazumevano<string>()` када је метода
дефинисана као `where T : struct`?
```

## Питање 3

```{mchoice}
:answer1: Да T има бар једну методy new
:answer2: Да T имплементира интерфејс INew
:answer3: Да T има јавни конструктор без параметара
:answer4: Да T буде вредносни тип
:correct: 3

Шта обезбеђује ограничење `where T : new()`?
```

## Питање 4

```{mchoice}
:answer1: int
:answer2: bool
:answer3: string
:answer4: double
:correct: 3

Који тип може бити прослеђен методи `IspisiIme<T>(T obj)` ако је дефинисана као
`where T : class`?
```

## Питање 5

```{mchoice}
:answer1: where T : class Vozilo
:answer2: where T : new Vozilo
:answer3: where T : Vozilo
:answer4: where T : IVozilo
:correct: 3

Која од наведених дефиниција омогућава да T буде само класа `Vozilo` или њене
изведене класе?
```

## Питање 6

```{mchoice}
:answer1: Метода ће се извршити без проблема
:answer2: Грешка ће се појавити у току извршавања
:answer3: Биће аутоматски извршено кастовање
:answer4: Десиће се грешка приликом компајлирања
:correct: 4

Шта ће се десити ако покушаш да проследиш класу `Bicikl` методи дефинисаној као
`where T : Vozilo`, а `Bicikl` није изведена из `Vozilo`?
```

## Питање 7

```{mchoice}
:answer1: where T : class, IIspis()
:answer2: where T : IIspis
:answer3: where T implements IIspis
:answer4: where T is IIspis
:correct: 2

Како се дефинише метод који прима тип T који мора да имплементира интерфејс
`IIspis`?
```

## Питање 8

```cs
public static void KojaJeVelicina<T, U>(T velicina) where T : U
{
    // ...
}
```

```{mchoice}
:answer1: Тип U мора бити вредносни тип
:answer2: Тип T мора бити истог типа као и U или изведен из U
:answer3: T мора бити интерфејс, а U класа
:answer4: Типови морају бити исти
:correct: 2

Шта значи ограничење `where T : U` у дефиницији дате методе?
```

## Питање 9

```{mchoice}
:answer1: interface, class, new()
:answer2: struct, interface, new()
:answer3: class, interface, new()
:answer4: new(), interface, class
:correct: 3

Који је исправан редослед навођења у вишеструком ограничењу?
```

## Питање 10

```{mchoice}
:answer1: where T1 : ref where T2 : val
:answer2: where T1 : class, T2 : struct
:answer3: where T1 : class where T2 : struct
:answer4: where T1, T2 : struct
:correct: 3

Ако метода има два типска параметра T1 и T2, и желиш да T1 буде референтни, а
T2 вредносни тип, како се пишу ограничења?
```
