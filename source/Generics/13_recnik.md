# Речник

## Kолекција Dictionary

У програмском језику C#, генеричка колекција `Dictionary<TKey, TValue>`
представља структуру података која чува парове кључ–вредност и омогућава брз
приступ подацима на основу кључа. У лекцији
[Колекције података](07_kolekcije.md) поненуто је да је колекција речник у
програмском језику C# реализована као генеричка колекција података. Дефинисана
је у класи
[`Dictionary<TKey,TValue>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=netframework-4.8.1)
која се налази у именском простору `System.Collections.Generic`.

Слично као хешсет и ова колекција заснована је на имплементацији хеш табеле,
што значи да има веома ефикасну алгоритамску сложеност $Θ(1)$. С друге стране,
због употребе хеш табеле, ова колекција може заузети више меморије у поређењу
са другим структурама података.

Сваки елемент у колекцији `Dictionary<TKey, TValue>` је пар кључа (`TKey`) и
вредности (`TValue`). Кључеви морају бити јединствени у оквиру једног речника,
а у случају додавања већ постојећег кључа баца се изузетак `ArgumentException`.
`TKey` и `TValue` су генерички типови, што значи да можеш користити било који
тип за кључ и вредност.

Елементи у речнику неће бити гарантовано поређани по редоследу додавања. Ако ти
је потребно одржавање редоследа, можеш користити генеричку колекцију
[SortedDictionary<TKey, TValue>](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sorteddictionary-2?view=netframework-4.8.1).

Неке од основних метода и својстава за рад са колекцијом речник су:

* `Add(TKey kljuc, TValue vrednost)`, додаје елемент са клучем `kljuc` и
вредношћу `vrednost` у речник,
* `Remove(TKey kljuc)`, уклања елемент са клучем `kljuc` из речника,
* `ContainsKey(TKey kljuc)`, проверава да ли речник садржи елемент са кључем
`kljuc`,
* `ContainsValue(TValue vrednost)`, проверава да ли речник садржи елемент са
вредношћу `vrednost`,
* `Count`, враћа укупан број елемената у речнику и
* `Clear()`, уклања све елементе из речника.

У следећем примеру...

```cs
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        Dictionary<string, int> ocene = new Dictionary<string, int>();
        ocene.Add("Paja", 5);
        ocene.Add("Raja", 4);
        ocene.Add("Gaja", 2);
        ocene.Add("Vlaja", 3);

        Console.WriteLine("Ukupno je dato {0} ocena", ocene.Count);

        if (ocene.ContainsKey("Paja"))
        {
            Console.WriteLine("Paja je dobio ocenu {0}", ocene["Paja"]);
        }

        ocene.Remove("Paja");

        foreach (var element in ocene)
        {
            Console.WriteLine("Ucenik {0} dobio je ocenu {1}", element.Key, element.Value);
        }

        ocene.Clear();
    }
}
```

...креира се речник `ocene` где су кључеви типа `string` (имена ученика) и
вредности типа `int` (њихове оцене). Потом се додају елементи речника, исписује
број елемената у речнику, проверава се да ли постоји кључ `Paja` и исписују
његове оцене, брише се елемент са кључем `Paja` из речника, па се исписују
преостали ученици и њихове оцене. На крају су обрисани сви елементи из речника.

Извршавањем овог програма у конзоли ће се исписати:

```text
Ukupno je dato 4 ocena
Paja je dobio ocenu 5
Ucenik Raja dobio je ocenu 4
Ucenik Gaja dobio je ocenu 2
Ucenik Vlaja dobio je ocenu 3
```

## Колекција SortedDictionary

У програмском језику C#, генеричка колекција сортирани речник
`SortedDictionary<TKey, TValue>` представља структуру података која
чува парове кључ–вредност у којoj су елементи сортирани по кључу. У лекцији
[Колекције података](07_kolekcije.md) поненуто је да је колекција сортирани
речник у програмском језику C# реализована као генеричка колекција података.
Дефинисана је у класи
[`SortedDictionary<TKey,TValue>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.sorteddictionary-2?view=netframework-4.8.1)
која се налази у именском простору `System.Collections.Generic`. У односу на
колекцију речник, сортирани речник не користи хеш табелу већ балансирано
бинарно стабло (енгл. *Red-Black Tree*), што проузрокује логаритамску сложеност
$O(\log n)$ за операције уметања, брисања и претраге. Иако има нешто спорије
перформансе у односу на речник, ова генеричка колекција омогућава аутоматско
сортирање кључева.

Следећи пример је скоро идентичан као претходни...

```cs
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        SortedDictionary<string, int> ocene = new SortedDictionary<string, int>();
        ocene.Add("Paja", 5);
        ocene.Add("Raja", 4);
        ocene.Add("Gaja", 2);
        ocene.Add("Vlaja", 3);

        Console.WriteLine("Ukupno je dato {0} ocena", ocene.Count);

        if (ocene.ContainsKey("Paja"))
        {
            Console.WriteLine("Paja je dobio ocenu {0}", ocene["Paja"]);
        }

        ocene.Remove("Paja");

        foreach (var element in ocene)
        {
            Console.WriteLine("Ucenik {0} dobio je ocenu {1}", element.Key, element.Value);
        }

        ocene.Clear();
    }
}
```

...али ће се елементи у конзоли исписати сортирани по кључу:

```text
Ukupno je dato 4 ocena
Paja je dobio ocenu 5
Ucenik Gaja dobio je ocenu 2
Ucenik Raja dobio je ocenu 4
Ucenik Vlaja dobio je ocenu 3
```
