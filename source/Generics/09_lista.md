# Листа

Листа представља строго типизирану колекцију објеката којима се може приступити
путем индекса. У лекцији [Колекције података](07_kolekcije.md) поменуто је да
је колекција листа у програмском језику C# реализована као генеричка колекција
података. Генеричка листа дефинисана је у класи
[`List<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=netframework-4.8)
која се налази у именском простору `System.Collections.Generic`. Можеш је
посматрати као динамички низ елемената типа `T`, где `T` може бити било који
тип. За разлику од класичних низова (`array`), колекција `List<T>` може
динамички да мења своју величину. То значи да можеш додавати или уклањати
елементе без потребе за мануелном алокацијом меморије. Креирање генеричке листе
и рад са методама из класе `List<T>` су прилично једноставни и интуитивни.

## Додавање елемената

Листу можеш иницијализовати са вредностима одмах приликом декларације, на
пример:

```cs
List<string> ucenici = new List<string> { "Paja", "Raja", "Vlaja" };
```

У овом примеру иницијализована је листа `ucenici` са три елемента: `Paja`,
`Raja` и `Vlaja`.

Методом `Add(T element)` додаје се елемент `element` на крај листе, а методом
`AddRange(IEnumerable<T> kolekcija)` додаје се више елемената из колекције
`kolekcija` на крај листе. На пример:

```cs
List<string> novi = new List<string> { "Maja", "Zlaja" };
ucenici.Add("Gaja");
ucenici.AddRange(novi);
```

Овим је на претходно иницијализовану листу `ucenici` прво додат ученик `Gaja`,
па онда и колекција `novi`, односно ученици `Maja` и `Zlaja`. Сада листа
`ucenici` садржи шест елемената: `Paja`, `Raja`, `Vlaja`, `Gaja`, `Maja` и
`Zlaja`.

## Уклањање елемената

Методом `Remove(T element)` уклања се први елемент `element`, методом
`RemoveAt(int indeks)` уклања се елемент са индексом `indeks`, док се методом
`Clear()` уклањају сви елементи из листе. На пример:

```cs
ucenici.Remove("Raja");
ucenici.RemoveAt(1);
```

Овим примером прво је уклоњен елемент `Raja` који је био на индексу `1`, па
након тога елемент са индекса `1`, односно `Vlaja`. Уклањањем елемента `Raja`
са индекса `1`, на његово место аутоматски је дошао `Vlaja` и добио индекс `1`,
`Gaja` је добио индекс `2`, `Maja` индекс `3` и `Zlaja` индекс `4`. Након другог
уклањања листа има четири елемента: `Paja` са индексом `0`, `Gaja` са индексом
`1`, `Maja` са индексом `2` и `Zlaja` са индексом `3`.

## Приступање елементима

Елементима се може приступити преко индекса: нпр. `lista[0]` враћа први елемент
листе `lista`, а својство `Count` враћа број елемената у листи. Методом
`Contains(T element)` проверава се да ли листа садржи елемент `element`, а
методом `IndexOf(T element)` враћа се индекс првог појављивања елемента
`element`. На пример, ако тренутно листа `ucenici` има елементе `Paja`, `Gaja`,
`Maja` и `Zlaja`...

```cs
Console.WriteLine(ucenici[1]);
Console.WriteLine(ucenici.Count);
if (ucenici.Contains("Maja"))
{
    Console.WriteLine(ucenici.IndexOf("Maja"));
}
```

...у конзоли ће се исписати...

```text
Gaja
4
2
```

...јер је `Gaja` елемент са индексом `1`, у листи има четири елемента, и пошто
листа садржи елемент `Maja` исписаће се његов индекс `2`.

## Операције са листом

Елементи листе се могу сортирати методом `Sort()`, а методом `Reverse()` обрће
се редослед елемената у листи. На крају, методом `ToArray()` листа се
конвертује у низ. Након операција из претходног примера, листа и даље има
четири елемента: `Paja`, `Gaja`, `Maja` и `Zlaja`. Након извршења следећег
кода...

```cs
ucenici.Sort();
ucenici.Reverse();
Array niz = ucenici.ToArray();
```

...листа `ucenici` ће се прво сортирати лексикографски, па ће редослед
елемената у листи бити: `Gaja`, `Maja`, `Paja` и `Zlaja`. Након тога ће се
редослед обрнути:`Zlaja`, `Paja`, `Maja` и `Gaja`. На крају ће се формирати низ
`niz` који садржи четири стринга `Zlaja`, `Paja`, `Maja` и `Gaja`.

Приликом рада са `List<T>` колекцијама, често се користи метода
`ForEach(Action<T>)` и ламбда изрази. Тако, уместо методе...

```cs
foreach (string ucenik in ucenici)
{
    Console.WriteLine(ucenik);
}
```

...можеш написати...

```cs
ucenici.ForEach(ucenik => Console.WriteLine(ucenik));
```

...где ће и један и други кôд резултовати исписом елемената листе у конзоли,
ред по ред:

```text
Zlaja
Paja
Maja
Gaja
```

## Листе и објекти кориснички дефинисаних класа

Листа је вероватно најчешће коришћена генеричка колекција, где су често њени
елементи заправо објекти кориснички дефинисаних класа. На пример, ако је
дефинисана класа `Ucenik`...

```cs
class Ucenik
{
    public string Ime { get; set; }
    public int Razred { get; set; }

    public Ucenik(string ime, int razred)
    {
        Ime = ime;
        Razred = razred;
    }

    public override string ToString()
    {
        return $"{Ime} (razred: {Razred})";
    }
}
```

...креирање листе објеката класе `Ucenik` и рад са том листом може да изгледа
овако:

```cs
class Program
{
    static void Main()
    {
        List<Ucenik> ucenici = new List<Ucenik>();
        ucenici.Add(new Ucenik("Paja", 1));
        ucenici.Add(new Ucenik("Raja", 2));
        ucenici.Add(new Ucenik("Vlaja", 3));
        ucenici.Add(new Ucenik("Maja", 2));
        
        Console.WriteLine("Spisak učenika:");
        foreach (Ucenik u in ucenici)
        {
            Console.WriteLine(u);
        }

        Console.WriteLine("\nUčenici drugog razreda:");
        foreach (Ucenik u in ucenici)
        {
            if (u.Razred == 2)
            {
                Console.WriteLine(u.Ime);
            }
        }
    }
}
```

Извршавањем овог програма у конзоли ће се исписати:

```text
Spisak ucenika:
Paja (razred: 1)
Raja (razred: 2)
Vlaja (razred: 3)
Maja (razred: 2)

Ucenici drugog razreda:
Raja
Maja
```

Класа `List<T>` омогућава рад са листама које се могу динамички проширивати или
смањивати. Подржава бројне методе за додавање, уклањање, претрагу и сортирање
елемената, што је чини веома корисном за рад са подацима у .NET Framework
апликацијама.
