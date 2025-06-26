# Квиз: ламбда изрази

## Питање 1

```{mchoice}
:answer1: Да дефинишу интерфејсе
:answer2: Да замене switch наредбе
:answer3: Да дефинишу анонимне функције кратким записом
:answer4: Да дефинишу генеричке типове података
:correct: 3

Која је основна сврха ламбда израза у програмском језику C#?
```

## Питање 2

```cs
Func<int, int> kvadrat = x => x * x;
Console.WriteLine(kvadrat(4));
```

```{mchoice}
:answer1: 4
:answer2: false
:answer3: 16
:answer4: true
:correct: 3

Шта је резултат извршавања датог кода?
```

## Питање 3

```{mchoice}
:answer1: => x + 1
:answer2: x => { return x + 1 }
:answer3: x => x + 1
:answer4: x -> x + 1
:correct: 3

Који од наведених ламбда израза је исправан?
```

## Питање 4

```{mchoice}
:answer1: (x + y) => x + y
:answer2: (x, y) => x + y
:answer3: (x, y) => return x + y;
:answer4: x, y => x + y
:correct: 2

Како изгледа ламбда израз који прима два параметра и сабира их?
```

## Питање 5

```{mchoice}
:answer1: Action
:answer2: Predicate
:answer3: Func
:answer4: EventHandler
:correct: 2

Који од наредних делегата се најчешће користи за ламбда израз који враћа `bool`?
```

## Питање 6

```cs
Predicate<int> paran = x => x % 2 == 0;
Console.WriteLine(paran(5));
```

```{mchoice}
:answer1: True
:answer2: False
:answer3: 0
:answer4: 2.5
:correct: 2

Шта је резултат извршавања датог кода?
```

## Питање 7

```{mchoice}
:answer1: Select
:answer2: Where
:answer3: OrderBy
:answer4: ToList
:correct: 2

Који метод колекције се користи за филтрирање података помоћу ламбда израза?
```

## Питање 8

```cs
List<int> brojevi = new List<int> { 1, 2, 3, 4 };
bool rezultat = brojevi.All(x => x > 0);
```

```{mchoice}
:answer1: true
:answer2: false
:answer3: 0
:answer4: 4
:correct: 1

Која ће вредност бити додељена променљивој `rezultat`?
```

## Питање 9

```cs
Func<int, int, int> m = (a, b) =>
{
    if (a > b) return a;
    else return b;
};
```

```{mchoice}
:answer1: Приказује већи од два дата броја
:answer2: Враћа true ако је први број већи
:answer3: Враћа увек први број
:answer4: Враћа већи од два дата броја
:correct: 4

Чему служи следећи кôд?
```

## Питање 10

```{mchoice}
:answer1: Када желиш да филтрираш елементе листе
:answer2: Када желиш да позовеш метод без параметара
:answer3: Када желиш да користиш goto унутар израза
:answer4: Када желиш да користиш Func делегат
:correct: 3

У којој ситуацији није могуће користити ламбда израз?
```
