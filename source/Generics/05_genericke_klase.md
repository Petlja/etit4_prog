# Генеричке класе

Слично као и генеричке методе, у програмском језику C# генеричке класе
омогућују дефинисање класа које могу да раде са било којим типом података, где
се конкретан тип одређује приликом коришћења класе.

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

...`GenerickaKlasa<T>` је генеричка класа са параметром `T`, који може бити
било који тип. У атрибуту `vrednost` чува се податак типа `T`, у конструктору
се прима вредност типа `T`, а методом `Prikazi()` та вредност се исписује у
конзоли.

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
