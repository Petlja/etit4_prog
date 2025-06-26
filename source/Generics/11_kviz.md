# Квиз: стек и ред

## Питање 1

```{mchoice}
:answer1: List Insert, First Output
:answer2: Last In, First Out
:answer3: Last Input, First Order
:answer4: Least Indexed, First Out
:correct: 2

Шта означава скраћеница LIFO која описује рад стека?
```

## Питање 2

```{mchoice}
:answer1: Add()
:answer2: Insert()
:answer3: Push()
:answer4: Append()
:correct: 3

Која метода се користи за додавање елемента на врх стека?
```

## Питање 3

```{mchoice}
:answer1: Уклониће последњи елемент
:answer2: Прочитаће елемент на дну стека
:answer3: Вратиће елемент са врха без уклањања
:answer4: Уклониће први елемент
:correct: 3

Шта ће метода `Peek()` урадити у `Stack<T>`?
```

## Питање 4

```{mchoice}
:answer1: Филтрирање података
:answer2: Памћење историје измена и undo/redo
:answer3: Индексирање података
:answer4: Приказ нумеричких података
:correct: 2

Која је типична примена стека у апликацијама?
```

## Питање 5

```{mchoice}
:answer1: Враћа null
:answer2: Враћа 0
:answer3: Баца изузетак
:answer4: Игнорише наредбу
:correct: 3

Шта се дешава када позовеш `Pop()` над празним стеком?
```

## Питање 6

```{mchoice}
:answer1: LIFO
:answer2: FILO
:answer3: FIFO
:answer4: LILO
:correct: 3

Који принцип следи структура података ред (queue)?
```

## Питање 7

```{mchoice}
:answer1: Push()
:answer2: Add()
:answer3: Insert()
:answer4: Enqueue()
:correct: 4

Која се метода користи за додавање елемента у ред?
```

## Питање 8

```{mchoice}
:answer1: Уклања последњи елемент из реда
:answer2: Уклања елемент са почетка реда
:answer3: Уклања елементе са вредношћу null
:answer4: Сортира елементе у реду
:correct: 2

Шта ради метода `Dequeue()`?
```

## Питање 9

```cs
Queue<string> red = new Queue<string>();
red.Enqueue("A");
red.Enqueue("B");
red.Dequeue();
```

```{mchoice}
:answer1: Исписује "A" и "B"
:answer2: Уклања "A" и враћа "B" као први елемент
:answer3: Уклања "B", а "A" остаје
:answer4: Не ради ништа јер ред је празан
:correct: 2

Шта ради дати кôд?
```

## Питање 10

```{mchoice}
:answer1: Не могу да се сортирају
:answer2: Не могу да се користе у петљама
:answer3: Не омогућавају приступ елементима преко индекса
:answer4: Захтевају више меморије
:correct: 3

Шта је заједничка "мана" стека и реда у односу на листу?
```
