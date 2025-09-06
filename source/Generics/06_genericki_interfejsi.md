# Генерички интерфејси

У програмском језику C#, генерички интерфејси омогућавају дефинисање интерфејса
који користе типске параметре, чиме се обезбеђују већа флексибилност и типска
безбедност у програму. Генеричке интерфејсе треба да користиш у ситуацијама
када желиш да дефинишеш заједничко понашање за различите типове података, а да
конкретни тип буде познат тек приликом имплементације.

## Дефиниција и имплементација генеричких интерфејса

Генерички интерфејс дефинише се слично као генеричка класа – навођењем типских
параметара након имена интерфејса:

```cs
interface IPrikazivac<T>
{
    void Prikazi(T vrednost);
}
```

У овом примеру, `IPrikazivac<T>` је генерички интерфејс са типским параметром
`T`. Интерфејс дефинише метод `Prikazi` који прима параметар типа `T`.

Класа која имплементира генерички интерфејс мора да наведе конкретан тип...

```cs
class PrikazivacString : IPrikazivac<string>
{
    public void Prikazi(string vrednost)
    {
        Console.WriteLine("String: " + vrednost);
    }
}
```

...или да и сама буде генеричка:

```cs
class GenerickiPrikazivac<T> : IPrikazivac<T>
{
    public void Prikazi(T vrednost)
    {
        Console.WriteLine("Vrednost: " + vrednost);
    }
}
```

У првом примеру, класа `PrikazivacString` имплементира интерфејс
`IPrikazivac<string>`, тј. има наведен конкретан тип `string`. У другом примеру
је и класа `GenerickiPrikazivac<T>` генеричка, и омогућава употребу било ког
типа за параметар `T`. Програм који користи ове класа може да изгледа овако:

```cs
class Program
{
    static void Main()
    {
        IPrikazivac<string> ps = new PrikazivacString();
        ps.Prikazi("Zdravo!");
        IPrikazivac<int> pi = new GenerickiPrikazivac<int>();
        pi.Prikazi(100);
    }
}
```

Извршавањем овог програма у конзоли ће се исписати:

```text
String: Zdravo!
Vrednost: 100
```

## Наслеђивање генеричких интерфејса

Један генерички интерфејс може да наслеђује други, као што класе могу да
наслеђују једна другу. На пример:

```cs
interface IObradjivac<T> : IPrikazivac<T>
{
    void Obradi(T vrednost);
}
```

Класа која имплементира `IObradjivac<T>` мора да имплементира оба метода:

```cs
class Procesor<T> : IObradjivac<T>
{
    public void Prikazi(T vrednost)
    {
        Console.WriteLine("Prikazujem: " + vrednost);
    }

    public void Obradi(T vrednost)
    {
        Console.WriteLine("Obradjujem: " + vrednost);
    }
}
```

## Унапред дефинисани генерички интерфејси

У .NET Framework-у постоје бројни уграђени генерички интерфејси. На пример:

* `IComparable<T>` – интерфејс који омогућава поређење објеката,
* `IEnumerable<T>` – интерфејс који омогућава итерирање кроз колекцију,
* `IEqualityComparer<T>` – интерфејс за поређење једнакости.

Списак и опис свих унапред дефинисаних генеричких интерфејса у програмском
језику C# можеш пронаћи у званичној
[документацији](https://learn.microsoft.com/en-us/dotnet/api/system?view=netframework-4.8#interfaces)
