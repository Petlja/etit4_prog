# Хешсет

Хешсет представља неуређен скуп јединствених елемената, што значи да не
дозвољава дупликате и не чува редослед уноса елемената. У лекцији
[Колекције података](07_kolekcije.md) поменуто је да је колекција хешсет у
програмском језику C# реализована као генеричка колекција података. Дефинисана
је у класи
[`HashSet<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.hashset-1?view=netframework-4.8.1)
која се налази у именском простору `System.Collections.Generic`.

Генеричка колекција `HashSet<T>` је оптимизована за брзе операције додавања,
уклањања и претраживања елемената. У просечном случају има алгоритамску
сложеност $Θ(1)$, што је чини веома ефикасном. Ове перформансе су могуће
захваљујући њеној интерној структури – хеш табели. Хеш табела је структура
података која мапира елементе на одређене позиције, тзв. *канте* (енгл.
*buckets*), користећи хеш функције. Када додајеш елемент у хешсет, C# позива
методу `GetHashCode()` тог елемента да би добио његов хеш кôд. Добијени хеш кôд
се затим користи за израчунавање индекса у табели где ће елемент бити смештен,
нпр. ако је добијени хеш кôд `12345`, а табела има 10 *канти*, индекс може бити
$12345 \mod 10 = 5$, што значи да се елемент смешта у канту са индексом `5`.
Ако је канта празна, операција је завршена. Ако већ постоји неки елемент, или
више њих, на том индексу, користи се додатна структура унутар те канте, нпр.
повезана листа, за чување свих елемената са истим хеш кодом. Да би била
осигурана јединственост, хешсет упоређује нови елемент са постојећим елементима
у истој канти користећи `Equals()`. Ако елемент већ постоји, не додаје га.

Приликом уклањања или претраживања елемената у хешсету, израчунава се хеш кôд
траженог елемента, проналази одговарајућа канта, па се проверавају само
елементи у тој канти. Овај поступак значајно смањује број поређења у односу на
линеарну претрагу, где је алгоритамска сложеност $Θ(n)$. У идеалном случају,
хеш функција распоређује елементе равномерно по кантама, тако да свака канта
садржи само један или неколико елемената. Пошто је приступ канти преко хеш кода
константан, а број елемената у канти је мали, 0 или 1, укупно време операције
је независно од укупног броја елемената у хешсету – отуда $Θ(1)$. Међутим, у
најгорем случају, због колизија, сложеност може порасти на $Θ(n)$ јер се
елементи тада складиште у линеарну структуру – листу повезаних елемената у
истој канти.

Неке од основних метода и својстава за рад са колекцијом хешсет су:

* `Add(T element)`, додаје елемент `element` у хешсет,
* `Remove(T element)`, уклања елемент `element` из хешсета,
* `Contains(T element)`, проверава да ли хешсет садржи елемент `element`,
* `Count`, враћа укупан број елемената у хешсету, и
* `Clear()`, уклања све елементе из хешсета.

```cs
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        HashSet<int> brojevi = new HashSet<int>();
        
        brojevi.Add(10);
        brojevi.Add(20);
        brojevi.Add(30);
        brojevi.Add(10); // Nece se dodati jer vec postoji!
        
        foreach (int broj in brojevi)
        {
            Console.WriteLine(broj);
        }
        
        if (brojevi.Contains(20))
        {
            Console.WriteLine("Broj 20 postoji nalazi se u hessetu!");
        }
        
        brojevi.Remove(10);
        foreach (int broj in brojevi)
        {
            Console.WriteLine(broj);
        }
    }
}
```

Извршавањем овог програма у конзоли ће се исписати:

```text
10
20
30
Broj 20 postoji nalazi se u hessetu!
20
30
```

Хешсет подржава операције над скуповима, као што су унија, пресек, разлика,
подскуп, надскуп и др. Методом `UnionWith(IEnumerable<T> drugi)` можеш да
комбинујеш елементе тренутног скупа са елементима другог скупа, елиминишући
дупликате ($skupA \cup skupB$), па ће извршавање следећег програма...

```cs
static void Main()
{
    HashSet<int> skupA = new HashSet<int> { 1, 2, 3, 4 };
    HashSet<int> skupB = new HashSet<int> { 3, 4, 5, 6 };        
    skupA.UnionWith(skupB);
    foreach(int element in skupA)
    {
        Console.Write("{0} ",element);
    }
}
```

...резултовати следећим исписом у конзоли:

```text
1 2 3 4 5 6
```

Слично, методом `IntersectWith(IEnumerable<T> drugi)` можеш да модификујеш
тренутни скуп тако да садржи само елементе који су присутни и у тренутном скупу
и у другом скупу ($skupA \cap skupB$), па ће извршавање следећег програма...

```cs
static void Main()
{
    HashSet<int> skupA = new HashSet<int> { 1, 2, 3, 4 };
    HashSet<int> skupB = new HashSet<int> { 3, 4, 5, 6 };        
    skupA.IntersectWith(skupB);
    foreach(int element in skupA)
    {
        Console.Write("{0} ",element);
    }
}
```

...резултовати следећим исписом у конзоли:

```text
3 4
```

Методом `SymmetricExceptWith(IEnumerable<T> drugi)` можеш да модификујеш
тренутни скуп тако да садржи само елементе који су јединствени за један од два
скупа, односно да уклониш елементе који се појављују у оба скупа
($skupA \triangle skupB$), па ће извршавање следећег програма...

```cs
static void Main()
{
    HashSet<int> skupA = new HashSet<int> { 1, 2, 3, 4 };
    HashSet<int> skupB = new HashSet<int> { 3, 4, 5, 6 };        
    skupA.SymmetricExceptWith(skupB);
    foreach(int element in skupA)
    {
        Console.Write("{0} ",element);
    }
}
```

...резултовати следећим исписом у конзоли:

```text
1 2 6 5
```

Методом `ExceptWith(IEnumerable<T> drugi)` можеш да из тренутног скупа уклониш
све елементе који су присутни у другом скупу ($skupA \setminus skupB$), па ће
извршавање следећег програма...

```cs
static void Main()
{
    HashSet<int> skupA = new HashSet<int> { 1, 2, 3, 4 };
    HashSet<int> skupB = new HashSet<int> { 3, 4, 5, 6 };        
    skupA.ExceptWith(skupB);
    foreach(int element in skupA)
    {
        Console.Write("{0} ",element);
    }
}
```

...резултовати следећим исписом у конзоли:

```text
1 2
```

Методом `IsSubsetOf(IEnumerable<T> drugi)` можеш да провериш да ли је тренутни
скуп подскуп другог скупа ($skupA \subset skupB$), па ће извршавање следећег
програма...

```cs
static void Main()
{
    HashSet<int> skupA = new HashSet<int> { 1, 2, 3, 4 };
    HashSet<int> skupB = new HashSet<int> { 3, 4 };
    bool nadskup1 = skupA.IsSubsetOf(skupB);
    bool nadskup2 = skupB.IsSubsetOf(skupA);
    Console.WriteLine("{0}, {1}", nadskup1, nadskup2);
}
```

...резултовати следећим исписом у конзоли...

```text
False, True
```

...јер скуп `skupA` није подскуп скупа `skupB`, односно, скуп `skupB` јесте
подскуп скупа `skupA`.

Методом `IsSupersetOf(IEnumerable<T> drugi)` можеш да провериш да ли је
тренутни скуп надскуп другог скупа ($skupA \supset skupB$), па ће извршавање
следећег програма...

```cs
static void Main()
{
    HashSet<int> skupA = new HashSet<int> { 1, 2, 3, 4 };
    HashSet<int> skupB = new HashSet<int> { 3, 4 };        
    bool nadskup1 = skupA.IsSupersetOf(skupB);
    bool nadskup2 = skupB.IsSupersetOf(skupA);
    Console.WriteLine("{0}, {1}", nadskup1, nadskup2);
}
```

...резултовати следећим исписом у конзоли...

```text
True, False
```

...јер скуп `skupA` јесте надскуп скупа `skupB`, односно, скуп `skupB` није
надскуп скупа `skupA`.
