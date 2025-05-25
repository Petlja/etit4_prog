# Ламбда изрази

Ламбда изрази представљају кратак начин да се напишу анонимне функције у
програмском језику C#. Користе се када је потребно проследити блок кода као
аргумент некој методи, а нарочито су корисни у раду са колекцијама и
делегатима. Основна синтакса ламбда израза је:

```cs
(parametri) => izjava_ili_blok_koda
```

Уколико је израз једноставан и враћа вредност, витичасте заграде и `return`
могу се изоставити.

Ламбда изрази могу имати:

| Облик                         | Опис                                   |
| ----------------------------- | -------------------------------------- |
| `() => Console.WriteLine()`   | без параметара                         |
| `x => x + 1`                  | један параметар, без заграда           |
| `(x, y) => x + y`             | више параметара                        |
| `(x, y) => { return x + y; }` | тело са више изјава, укључује `return` |

## Делегати и ламбда изрази

Ламбда изрази се често користе уместо делегата. На пример:

```cs
static void Main()
{
    Func<int, int> kvadrat = x => x * x;
    Console.WriteLine(kvadrat(6));
}
```

У овом примеру, `Func<int, int>` представља делегат који прима `int` и враћа
`int`. Кроз ламбда израз `x => x * x` дефинисана је функција која враћа квадрат
броја. Еквивалентан програм без ламбда израза може да изгледа овако...

```cs
static int Kvadrat(int x)
{
    return x * x;
}

static void Main()
{
    Func<int, int> kvadrat = Kvadrat;
    Console.WriteLine(kvadrat(6));
}
```

...или без `Func` делегата овако:

```cs
static int Kvadrat(int x)
{
    return x * x;
}

static void Main()
{
    Console.WriteLine(Kvadrat(6));
}
```

У првом примеру користи се делегат `Func<int, int>` и експлицитно дефинисана
метода `Kvadrat`, док се у другом директно позива метода. Када упоредиш ова два
решења са еквивалентним ламбда изразом, одмах ће ти бити јасно зашто су ламбда
изрази популарни и зашто доприносе прегледности и скраћивању обима кода:

* краћи су и читљивији,
* не захтевају посебну декларацију методе и
* могу се прослеђивати директно као аргументи.

Ламбда изрази се могу користити и са другим генеричким делегатима, на пример:

```cs
Action<string> pozdravi = ime => Console.WriteLine("Zdravo, " + ime);
pozdravi("Tamara"); // Zdravo, Tamara

Predicate<int> paran = x => x % 2 == 0;
Console.WriteLine(paran(5)); // False
```

## Колекције и ламбда изрази

Ламбда изрази се најчешће користе са методама као што су `Where`, `Select`,
`Find`, `Any`, `All`, `OrderBy`, итд., за филтрирање, мапирање, сортирање и за
трансформацију података. На пример:

```cs
List<int> brojevi = new List<int> { 1, 2, 3, 4, 5 };

// Izdvajanje parnih brojeva
List<int> parni = brojevi.Where(x => x % 2 == 0).ToList();

// Pretvaranje u kvadrate brojeva
List<int> kvadrati = brojevi.Select(x => x * x).ToList();

// Provera da li postoji broj veći od 3
bool postojiVeciOdTri = brojevi.Any(x => x > 3); // true

// Provera da li su svi brojevi pozitivni
bool sviPozitivni = brojevi.All(x => x > 0); // true

// Pronalazak prvog broja koji je deljiv sa 3
int prviDeljivSaTri = brojevi.Find(x => x % 3 == 0); // 3

// Sortiranje brojeva opadajuće
List<int> opadajuce = brojevi.OrderByDescending(x => x).ToList(); // [5, 4, 3, 2, 1]

// Sortiranje brojeva rastuće
List<int> rastuce = brojevi.OrderBy(x => x).ToList(); // [1, 2, 3, 4, 5]
```

Ламбда изрази имају ограничења, тј. не смеју да садрже оператор `goto`, `break`
или `continue` ако нису унутар петље!

## Генеричке методе и ламбда изрази

Ламбда изразе можеш користити и приликом рада са генеричким методама. На
пример:

```cs
public static T GenerickaMetoda<T>(T vrednost, Func<T, T> funkcija)
{
    return funkcija(vrednost);
}

static void Main()
{
    var rezultat = GenerickaMetoda(5, x => x * 2);
    Console.WriteLine(rezultat);
}
```

Овај пример демонстрира генеричку методу која прима произвољну вредност и
трансформише је помоћу ламбда израза. `T` је генерички тип што значи да
метода ради са било којим типом, `vrednost` је улазна вредност која треба да
се трансформише, `Func<T, T> funkcija` је делегат који прима један аргумент
типа `T` и враћа резултат типа `T` и `return funkcija(vrednost)` примењује
прослеђену функцију на `vrednost` и враћа резултат. Позивом методе
`GenerickaMetoda` у методи `Main()` прослеђује се цео број и ламбда израз
који дуплира број. `GenerickaMetoda` прима `5`, примењује `x * 2` и враћа `10`.

## Вишепараметарски ламбда изрази

Ламбда изрази могу примати више параметара...

```cs
Func<int, int, int> sabiranje = (a, b) => a + b;
Console.WriteLine(sabiranje(3, 4)); // 7
```

...а ако је потребно више линија кода, користе се витичасте заграде:

```cs
Func<int, int, int> maksimum = (a, b) =>
{
    if (a > b)
    {
        return a;
    }
    else
    {
        return b;
    }
};
Console.WriteLine(maksimum(10, 20)); // 20
```

Значи, ламбда изразе можеш да користиш када треба да проследиш кратку
логику као аргумент методи, радиш са делегатима или догађајима, или желиш да
избегнеш дефинисање засебне методе за једноставан израз.
