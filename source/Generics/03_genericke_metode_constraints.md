# Ограничења за типске параметре

Приликом дефиниције генеричке методе, можеш поставити и ограничења за типске
параметре (енгл. *constraints*), како би имао бољу контролу над аргументима
који се прослеђују. Ова ограничења задају се помоћу кључне речи `where`, а могу
бити:

* ограничења на вредносне типове,
* ограничења на референтне типове,
* ограничење на постојање подразумеваног конструктора,
* ограничење на одређену класу,
* ограничење на одређени интерфејс,
* ограничење на други типски параметар и
* вишеструка ограничења.

## Ограничење на вредносне типове

О вредносним типовима учио си у
[III разреду](https://petlja.org/sr-Cyrl-RS/kurs/14469/2/9846). Типски
параметар може да се ограничи на вредносне типове навођењем `where T : struct`.
На пример, нека је задатак да дефинишеш методу `Podrazumevano` која ће у
конзоли исписивати подразумеване вредности вредносних типова који јој се
прослеђују као аргументи. Решење овог задатка може да изгледа овако:

```cs
public static void Podrazumevano<T>() where T : struct
{
    Console.WriteLine(default(T));
}
```

Извршавањем следећег програма у којем се позива методе `Podrazumevano`...

```cs
static void Main()
{
    Podrazumevano<int>();
    Podrazumevano<bool>();
    Podrazumevano<DateTime>();
}
```

...у конзоли ће се исписати подразумеване вредности наведених вредносних
типова:

```text
0
False
1/1/0001 12:00:00 AM
```

Метода се не може позвати овако...

```cs
Podrazumevano<string>();
```

...јер је `string` референтни тип, те ће такав позив изазвати грешку приликом
компајлирања:

```text
CS0453: The type 'string' must be a non-nullable value type in order to use it
as parameter 'T' in the generic type or method 'Program.Podrazumevano<T>()'
```

## Ограничење на референтне типове

О референтним типовима такође си учио у
[III разреду](https://petlja.org/sr-Cyrl-RS/kurs/14469/2/9848).
Типски параметар може да се ограничи на референтне типове навођењем
`where T : class`. На пример, нека је задатак да дефинишеш методу `IspisiIme`
која ће у конзоли исписати име вредносног типа који јој се прослеђује као
аргумент. Решење овог задатка може да изгледа овако:

```cs
public static void IspisiIme<T>(T objekat) where T : class
{
    Console.WriteLine(objekat.GetType().Name);
}
```

Извршавањем следећег програма у којем се позива методе `IspisiIme`...

```cs
static void Main()
{
    int[] arr = new int[] { 5, 1, 9, 7, 0, 3, 6, 8, 4, 2 }; ;
    IspisiIme(arr);
    IspisiIme("Pozdrav!");
}
```

...у конзоли ће се исписати имена наведених референтних типова:

```text
Int32[]
String
```

Метода се не може позвати овако...

```cs
IspisiIme(5);
```

...јер је целобројни податак `5` вредносног типа `int`, те ће такав позив
изазвати грешку приликом компајлирања:

```text
CS0452: The type 'int' must be a reference type in order to use it as parameter
'T' in the generic type or method 'Program.IspisiIme<T>(T)''
```

## Ограничење на постојање подразумеваног конструктора

О подразумеваном конструктору учио си у
[III разреду](https://petlja.org/sr-Cyrl-RS/kurs/14469/3/9874).
Типски параметар може да се ограничи на типове који имају јавни конструктор без
параметара, тј. подразумевани конструктор, навођењем `where T : new()`. То
значи да следеће неће бити дозвољено:

* апстрактне класе, јер не могу имати инстанце,
* интерфејси, јер немају конструкторе,
* структуре без експлицитног конструктора, јер свака структура увек има
подразумевани јавни конструктор који не прихвата параметре, али ако желиш да
користиш `new()` ограничење, тај конструктор треба да буде доступан, што може
бити проблем у специфичним случајевима и
* класе које немају јавни конструкор без параметара, ако класа има само
приватне или заштићене конструкторе, неће моћи да се користи.

Ово ограничење је корисно када треба да направиш нови објекат типа `T` унутар
методе, односно када креираш класе које динамички креирају објекте. У следећем
једноставном примеру...

```cs
class MojaKlasa { }

class Program
{
    public static T KreirajInstancu<T>() where T : new()
    {
        return new T();
    }

    static void Main()
    {
        var objekat = KreirajInstancu<MojaKlasa>();
    }
}
```

...метода `KreirajInstancu` је генеричка и враћа објекат типа `T`, а
`where T : new()` осигурава да `T` има подразумевани конструктор. Унутар методе
користи се `new T()` јер је компајлер сигуран да `T` има такав конструктор. У
методи `Main` позива се `KreirajInstancu<MojaKlasa>()`, а пошто `MojaKlasa` има
подразумевани конструктор, све функционише без грешке.

## Ограничење на одређену класу

Типски параметар `T` може да се ограничи и на одређену базну класу и њене
изведене класе. На пример, ако желиш да ограничиш `T` да мора бити класе
`Vozilo` или њених изведених класа, генеричка метода у класи `Primer` може да
изгледа овако:

```cs
class Vozilo { }

class Automobil : Vozilo { }

class Bicikl { }

class Primer
{
    public static void Ispisi<T>(T objekat) where T : Vozilo
    {
        Console.WriteLine(objekat.GetType().Name);
    }
}
```

У овом примеру `T` може бити базне класе `Vozilo` или изведене класе
`Automobil`. Извршавањем следећег програма...

```cs
class Program
{
    static void Main()
    {
        Primer.Ispisi(new Vozilo());
        Primer.Ispisi(new Automobil());
    }
}
```

...у конзоли ће се исписати:

```text
Vozilo
Automobil
```

У овом примеру `T` не може бити класе `Bicikl` или класе `Trotinet`, јер те
класе нису изведене из класе `Vozilo`. Следећа наредба...

```cs
Primer.Ispisi(new Bicikl());
```

...проузроковала би грешку приликом компајлирања:

```text
CS0311: The type 'Bicikl' cannot be used as type parameter 'T' in the generic
type or method 'Primer.Ispisi<T>(T)'. There is no implicit reference conversion
from 'Bicikl' to 'Vozilo'.
```

## Ограничење на одређени интерфејс

Када се користе генеричке методе, често је потребно да се осигура да прослеђени
тип имплементира одређени интерфејс, што се постиже навођењем
`where T : INekiIntefejs`.

Нека је потребно поставити ограничење да прослеђени тип имплементира интерфејс
`IIspis`. У следећем примеру само класа `Automobil` имплементира интерфејс
`IIspis` и има дефинисану методу `Ispisi`...

```cs
interface IIspis
{
    void Ispisi();
}

class Automobil : IIspis
{
    public void Ispisi()
    {
        Console.WriteLine("Automobil");
    }
}

class Bicikl
{
    public void Ispisi()
    {
        Console.WriteLine("Bicikl");
    }
}
```

...па ће се позивом генеричке методе `Ispisivanje` са проследећеним типом
класе `Automobil`...

```cs
class Program
{
    public static void IspisiTip<T>(T objekat) where T : IIspis
    {
        Console.WriteLine(objekat.GetType().Name);
    }

    static void Main()
    {
        IspisiTip(new Automobil());
    }
}
```

...у конзоли исписати:

```text
Automobil
```

Иако је у класи `Bicikl` дефинисана метода `Ispisi`, тип класе `Bicikl` не може
се проследити јер нема имплементиран интерфејс `IIspis`. Извршавање команде...

```cs
IspisiTip(new Bicikl());
```

...проузроковало би исту грешку приликом компајлирања, као и у претходном
примеру.

## Ограничење на други типски параметар

Типски параметар `T` може се ограничити на други типски параметар `U` или
његове изведене типове. Каже се да је `T` параметар који се ограничава, док је
`U` параметар који служи као ограничење. Овакво ограничење је корисно када
треба да се осигурају везе између типских параметара.

На пример, нека су дефинисани следећи интерфејс и класе:

```cs
public interface IStruja
{
    void IspisiVelicinu();
}

public class Napon : IStruja
{
    public void IspisiVelicinu()
    {
        Console.WriteLine("Volt [V]");
    }
}

public class Intenzitet : IStruja
{
    public void IspisiVelicinu()
    {
        Console.WriteLine("Amper [A]");
    }
}
```

Генеричка метода са ограничењем на други типски параметар и позиви такве методе
могу да изгледају овако...

```cs
class Program
{
    public static void KojaJeVelicina<T, U>(T velicina) where T : U
    {
        if (velicina is IStruja s)
        {
            s.IspisiVelicinu();
        }
    }

    static void Main(string[] args)
    {
        Napon napon = new Napon();
        KojaJeVelicina<Napon, IStruja>(napon);
        Intenzitet intenzitet = new Intenzitet();
        KojaJeVelicina<Intenzitet, IStruja>(intenzitet);
    }
}
```

...те ће се извршавањем овог програма у конзоли исписати:

```text
Volt [V]
Amper [A]
```

## Вишеструка ограничења

Генеричке методе могу имати вишеструка ограничења (енгл.
*multiple constraints*) за типске параметре. То значи да можеш да примениш више
ограничења на један типски параметар, како би осигурао да тип задовољава више
услова истовремено. Вишеструка ограничења такође се задају се помоћу кључне
речи `where`, где се свако ограничење одваја запетом. Редослед навођења
ограничења јесте битан:

* прво се наводи ограничење за тип (нпр. `class` или `struct`),
* затим за интерфејсе (нпр. `IMojInterfejs`) и
* на крају за конструктор (`new()`).

```cs
public void MojaMetoda<T>(T vrednost) where T : class, IMojInterfejs, new()
{
    // Telo metode
}
```

Такође, један тип може да имплементира више интерфејса...

```cs
public void MojaMetoda<T>(T vrednost) where T : IPrviInterfejs, IDrugiInterfejs
{
    // Telo metode
}
```

...али не може истовремено бити и вредносни и референтни тип тј. не можеш
комбиновати услове `class` и `struct` јер су међусобно контрадикторни.

На пример, нека постоје следеће дефиниције:

```cs
public interface IObavestenja
{
    void Obavesti(string poruka);
}

public class BaznaKlasa
{
    public string Ime { get; set; }
}

public class MojaKlasa : BaznaKlasa, IObavestenja
{
    public MojaKlasa()
    {
        Ime = "Podrazumevano ime";
    }

    public void Obavesti(string poruka)
    {
        Console.WriteLine("Naziv: " + poruka);
    }
}
```

Исправно дефинисана генеричка метода са вишеструким ограничењима и њен позив
могу да изгледају овако:

```cs
class Program
{
    public static void MojaMetoda<T>(T stavka) where T : BaznaKlasa, IObavestenja, new()
    {
        stavka.Obavesti(stavka.Ime);
        T novaInstanca = new T();
        novaInstanca.Ime = "Novo ime";
        novaInstanca.Obavesti(novaInstanca.Ime);
    }

    static void Main()
    {
        MojaKlasa objekat = new MojaKlasa();
        MojaMetoda(objekat);
    }
}
```

Извршавањем овог програма у конзоли ће се исписати:

```text
Naziv: Podrazumevano ime
Naziv: Novo ime
```

## Ограничења на више типских параметара

Када генеричка метода има више типских параметара, на сваки појединачни типски
параметар можеш да поставиш ограничења наводећи кључну реч `where`. На пример,
нека први типски параметар мора да буде референтни, а други вредносни тип:

```cs
public static void IspisiVrednosti<T1, T2>(T1 v1, T2 v2)
    where T1 : class
    where T2 : struct
{
    Console.WriteLine("{0}, {1}", v1, v2);
}
```

Исправан позив ове методе могао би да изгледа овако...

```cs
static void Main()
{
    IspisiVrednosti("Zdravo!", 5);
}
```

...а извршавањем овог програма у конзоли би се исписало:

```text
Zdravo!, 5
```
