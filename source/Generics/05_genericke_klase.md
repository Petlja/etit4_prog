# Генеричке класе

Слично као и генеричке методе, у програмском језику C# генеричке класе
омогућују дефинисање класа које могу да раде са било којим типом података, где
се конкретан тип одређује приликом коришћења класе. Значи, генеричке класе
елиминишу потребу за дуплирањем кода и пружају већу флексибилност и безбедност
приликом рада са различитим типовима података. На пример, уместо да креираш
посебне класе за `int`, `string`, `bool` итд., можеш користити једну генеричку
класу која ради са било којим типом.

## Дефиниција генеричке класе

Генеричка класа дефинише се навођењем типског параметра `<T>` након имена
класе. У следећем примеру...

```cs
class GenerickaKlasa<T>
{
    private T vrednost;

    public GenerickaKlasa(T vrednost)
    {
        this.vrednost = vrednost;
    }

    public void Prikazi()
    {
        Console.WriteLine("Vrednost: " + vrednost);
    }
}
```

...`GenerickaKlasa<T>` је генеричка класа са типским параметром `T`, који може
бити било који тип. У атрибуту `vrednost` чува се податак типа `T`, у
конструктору се прима вредност типа `T`, а методом `Prikazi()` та вредност се
исписује у конзоли.

## Генеричка поља у генеричкој класи

Генеричка поља су поља унутар генеричке класе која користе типске параметре те
класе. Ова поља омогућавају да се класа складишти и манипулише подацима било
ког типа, без потребе за кастовањем или коришћењем `object` типа.

У претходном примеру, поље...

```cs
private T vrednost;
```

представља генеричко поље у генеричкој класи у којем може да се чува вредност
било ког типа `T`. У овом примеру, вредност се додељује пољу `vrednost` кроз
конструктор, а методом `Prikazi` вредност истог типа `T` исписује се у конзоли.

## Инстанцирање генеричке класе

Када се генеричка класа инстанцира, мора се навести конкретан тип који замењује
параметар `T`. За претходно дефинисану генеричку класу `GenerickaKlasa`...

```cs
class Program
{
    static void Main()
    {
        GenerickaKlasa<int> gk1 = new GenerickaKlasa<int>(10);
        GenerickaKlasa<string> gk2 = new GenerickaKlasa<string>("Zdravo!");
        gk1.Prikazi();
        gk2.Prikazi();
    }
}
```

...инстанцирана су два објекта: `gk1` за тип `int` и `gk2` за тип `string`.
Извршавањем овог програма у конзоли ће се исписати:

```text
Vrednost: 10
Vrednost: Zdravo!
```

## Својства у генеричкој класи

У програмском језику C# својства не могу бити генеричка сама по себи, али могу
бити део генеричке класе, што значи да тип својства зависи од типског параметра
класе. На пример:

```cs
class GenerickaKlasa<T>
{
    public T VrednostSvojstva { get; set; }

    public GenerickaKlasa(T vrednost)
    {
        VrednostSvojstva = vrednost;
    }
}
```

## Ограничења типских параметара

Ограничења типских параметара за генеричке класе у програмском језику C#
дефинишу се на исти начин као и
[ограничења типских параметара за генеричке методе](03_genericke_metode_constraints.md),
користећи кључну реч `where`. Разлика је у томе што се код генеричких класа
ограничења примењују на типске параметре целе класе, док се код генеричких
метода примењују само на одређени метод.

Ако би задатак био да поставиш ограничење на типски параметар `T` у претходном
примеру, тако да `T` мора да буде референтни тип, класу би онда дефинисао
овако:

```cs
class GenerickaKlasa<T> where T : class
{
    private T vrednost;

    public GenerickaKlasa(T vrednost)
    {
        this.vrednost = vrednost;
    }

    public void Prikazi()
    {
        Console.WriteLine("Vrednost: " + vrednost);
    }
}
```

Инстанцирање објекта `gk1` из претходног примера

```cs
static void Main()
{
    GenerickaKlasa<int> gk1 = new GenerickaKlasa<int>(10);
    gk1.Prikazi();
}
```

изазвало би грешку приликом компајлирања:

```text
CS0452: The type 'int' must be a reference type in order to use it as parameter
'T' in the generic type or method 'GenerickaKlasa<T>'
```

јер је тип `int` вредносни, а не референтни тип, док би инстанцирање објекта
`gk2`...

```cs
static void Main()
{
    GenerickaKlasa<string> gk2 = new GenerickaKlasa<string>("Zdravo!");
    gk2.Prikazi();
}
```

....било успешно јер је тип `string` референтни тип, па би се у конзоли
исписало:

```text
Vrednost: Zdravo!
```

## Генеричке класе са више типских параметара

Слично као код генеричких метода и генеричке класе могу имати више типских
параметара који се наводе као `<T1, T2, ...>`, на пример:

```cs
class GenerickaKlasa<T1, T2>
{
    private T1 v1;
    private T2 v2;

    public GenerickaKlasa(T1 v1, T2 v2)
    {
        this.v1 = v1;
        this.v2 = v2;
    }

    public void Prikazi()
    {
        Console.WriteLine("{0}, {1}", v1, v2);
    }
}
```

Инстанцирање објеката ове класе може да изгледа овако...

```cs
class Program
{
    static void Main()
    {
        GenerickaKlasa<int, string> objekat1 = new GenerickaKlasa<int, string>(42, "Zdravo!");
        objekat1.Prikazi();
        GenerickaKlasa<bool, char> objekat2 = new GenerickaKlasa<bool, char>(true, 'A');
        objekat2.Prikazi();
    }
}
```

...а извршавањем овог програма у конзоли ће се исписати:

```text
42, Zdravo!
True, A
```

## Ограничења на више типских параметара

Генеричке класе са више типских параметара такође могу имати ограничења на
сваки појединачни типски параметар. На пример, генеричка класа која има два
типска параметра `T1` и `T2`, где `T1` мора бити референтни, а `T2` вредносни
тип, може да изгледа овако...

```cs
class GenerickaKlasa<T1, T2>
    where T1 : class
    where T2 : struct
{
    private T1 v1;
    private T2 v2;

    public GenerickaKlasa(T1 v1, T2 v2)
    {
        this.v1 = v1;
        this.v2 = v2;
    }

    public void Prikazi()
    {
        Console.WriteLine("{0}, {1}", v1, v2);
    }
}
```

...а инстанцирање објекта те класе овако:

```cs
class Program
{
    static void Main()
    {
        GenerickaKlasa<string, int> objekat = new GenerickaKlasa<string, int>("Zdravo!", 5);
        objekat.Prikazi();
    }
}
```

Извршавањем овог програма у конзоли ће се исписати:

```text
Zdravo!, 5
```

Генеричке класе у програмском језику C# представљају одличан механизам за
писање флексибилног и типски безбедног кода. Омогућавају креирање класа које се
могу користити са различитим типовима података, уз гаранцију да ће типови бити
проверени већ у току компајлирања. Комбинацијом са ограничењима, могуће је
још боље контролисати употребу генеричких типова и понашање класа.
