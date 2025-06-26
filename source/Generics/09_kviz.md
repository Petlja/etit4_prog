# Квиз: листе

## Питање 1

```{mchoice}
:answer1: Листа користи мање меморије
:answer2: Листа омогућава лексикографско сортирање
:answer3: Листа може динамички да мења величину
:answer4: Листа је увек бржа
:correct: 3

Која је главна предност листе у односу на класичне низове?
```

## Питање 2

```{mchoice}
:answer1: Додаје елемент на почетак листе
:answer2: Спаја две листе у једну
:answer3: Брише све елементе из листе
:answer4: Додаје више елемената одједном
:correct: 4

Шта ради метод `AddRange()` у класи `List<T>`?
```

## Питање 3

```{mchoice}
:answer1: Length
:answer2: Size()
:answer3: Count
:answer4: sizeof
:correct: 3

Која метода враћа број елемената у листи?
```

## Питање 4

```{mchoice}
:answer1: Обрће елементе
:answer2: Сортира елементе по абецеди
:answer3: Сортира елементе по индексу
:answer4: Сортира елементе по вредности
:correct: 2,4

Шта ради метод `Sort()` у `List<T>`?
```

## Питање 5

```{mchoice}
:answer1: radnici.Get(1)
:answer2: radnici.First()
:answer3: radnici[0]
:answer4: radnici.Index(0)
:correct: 3

Како можеш да приступиш првом елементу листе `radnici`?
```

## Питање 6

```{mchoice}
:answer1: radnici.DeleteAll()
:answer2: radnici.Clear()
:answer3: radnici.RemoveAll()
:answer4: radnici.Reset()
:correct: 2

Како можеш обрисати све елементе из листе `radnici`?
```

## Питање 7

```{mchoice}
:answer1: radnici.ForEach(radniк => Console.WriteLine(radniк));
:answer2: ForEach (radniк in radnici) Console.WriteLine(radniк);
:answer3: radnici.ForEach(x => return x);
:answer4: Console.WriteLine(=> ForEach(radniк in radnici));
:correct: 1

Како би исписао све раднике користећи `ForEach()` и ламбда израз?
```

## Питање 8

```{mchoice}
:answer1: Листа се може конвертовати у низ методом Convert()
:answer2: Листа се не може конвертовати у низ
:answer3: Методом Contains() проверава се да ли листа садржи одређени елемент
:answer4: Елементи листе не могу бити објекти кориснички дефинисаних класа
:correct: 3

Који је истинит исказ у вези генеричке колекције `List<T>`?
```

## Питање 9

```{mchoice}
:answer1: Console.WriteLine(radnici.Index("Paja"));
:answer2: Console.WriteLine(radnici.IndexOf("Paja"));
:answer3: Console.WriteLine(radnici.First("Paja"));
:answer4: Console.WriteLine(radnici.FindFirst("Paja"));
:correct: 2

Како би исписао индекс првог појављивања елемента `Paja` у листи `radnici`?
```

## Питање 10

```{mchoice}
:answer1: Тачно
:answer2: Нетачно
:correct: 2

Нема разлике у перформансама колекција ArrayList и List<T>.
```
