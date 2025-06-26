# Квиз: хешсет и речник

## Питање 1

```{mchoice}
:answer1: Чува елементе у редоследу у којем су додати.
:answer2: Дозвољава чување дупликата елемената.
:answer3: Омогућава веома брзе операције претраге, додавања и уклањања елемената.
:answer4: Гарантује да су елементи сортирани.
:answer5: Осигурава да су сви елементи у колекцији јединствени.
:correct: 3,5

Које од следећих тврдњи тачно описују `HashSet<T>` колекцију у C#?
```

## Питање 2

```{mchoice}
:answer1: Нова вредност ће преписати постојећу вредност за тај кључ.
:answer2: Add() метода ће вратити вредност false.
:answer3: Биће бачен изузетак ArgumentException.
:answer4: Нови пар кључ-вредност ће бити тихо игнорисан.
:correct: 3

Шта ће се догодити ако покушаш да додаш пар кључ-вредност у
`Dictionary<string, int>` користећи `Add()` методу, а кључ који додајеш већ
постоји у речнику?
```

## Питање 3

```{mchoice}
:answer1: SortedDictionary користи мање меморије.
:answer2: SortedDictionary има брже операције уметања и приступа елементима Θ(1).
:answer3: SortedDictionary чува елементе сортиране по кључу.
:answer4: SortedDictionary дозвољава дупликате кључева.
:correct: 3

Која је основна разлика и главни разлог за избор
`SortedDictionary<TKey, TValue>` у односу на `Dictionary<TKey, TValue>`?
```

## Питање 4

```cs
SortedDictionary<string, int> ocene = new SortedDictionary<string, int>();
ocene.Add("Paja", 5);
ocene.Add("Raja", 4);
ocene.Add("Gaja", 2);

foreach (var element in ocene)
{
    Console.Write("{0} ", element.Key);
}
```

```{mchoice}
:answer1: Paja Raja Gaja
:answer2: Gaja Paja Raja
:answer3: Gaja Raja Paja
:answer4: Редослед исписа није загарантован.
:correct: 2

Шта ће бити исписано у конзоли након извршавања датог кода?
```

## Питање 5

```{mchoice}
:answer1: skupA.UnionWith(skupB);
:answer2: skupA.IntersectWith(skupB);
:answer3: skupA.ExceptWith(skupB);
:answer4: skupA.SymmetricExceptWith(skupB);
:correct: 3

Десинисане су две `HashSet<int>` колекције: `skupA = {1, 2, 3}` и
`skupB = {3, 4, 5}`. Која операција ће резултовати тиме да `skupA` садржи само
елементе `{1, 2}`?
```

## Питање 6

```{mchoice}
:answer1: Хеш табела и Повезана листа
:answer2: Црвено-црно стабло и Хеш табела
:answer3: Повезана листа и Црвено-црно стабло
:answer4: Хеш табела и Црвено-црно стабло
:correct: 4

Које структуре података леже у основи имплементације `HashSet<T>` и
`SortedDictionary<TKey, TValue>` колекција?
```

## Питање 7

```{mchoice}
:answer1: ContainsValue(TValue value)
:answer2: Contains(TKey key)
:answer3: ContainsKey(TKey key)
:answer4: Exists(TKey key)
:correct: 3

Коју методу требаш користити да би проверио да ли `Dictionary<TKey, TValue>`
садржи одређени кључ пре покушаја приступа његовој вредности, како би се
избегао потенцијални изузетак?
```

## Питање 8

```{mchoice}
:answer1: O(log(n))
:answer2: Θ(n)
:answer3: Θ(n²)
:answer4: Θ(1)
:correct: 4

Која је просечна алгоритамска сложеност за операције `Add`, `Remove` и
`Contains` у `HashSet<T>` колекцији?
```

## Питање 9

```cs
HashSet<int> skupA = new HashSet<int> { 10, 20, 30 };
HashSet<int> skupB = new HashSet<int> { 20, 30, 40 };
skupA.SymmetricExceptWith(skupB);
foreach(int element in skupA)
{
    Console.Write("{0} ", element);
}
```

```{mchoice}
:answer1: 20 30
:answer2: 10 40
:answer3: 10 20 30 40
:answer4: 10
:correct: 2

Шта ће бити исписано у конзоли након извршавања датог кода?
```

## Питање 10

```{mchoice}
:answer1: Речник је заснован на хеш табели и нуди просечну сложеност Θ(1) за приступ елементима.
:answer2: Сортирани речник гарантује да се елементи исписују редоследом којим су убачени.
:answer3: Хешсет користи методе GetHashCode() и Equals() за управљање својим јединственим елементима.
:answer4: Метода Remove(TKey kljuc) у речнику враћа вредност елемента који је уклоњен.
:answer5: У најгорем случају, због колизија хеш кодова, сложеност операције Add у хешсету може бити Θ(n).
:correct: 1,3,5

Које од следећих тврдњи су тачне?
```
