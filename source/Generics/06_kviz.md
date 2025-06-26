# Квиз: генерички интерфејси

## Питање 1

```{mchoice}
:answer1: Статичку иницијализацију вредности
:answer2: Руковање искључиво вредносним типовима
:answer3: Флексибилност и типску безбедност при раду са различитим типовима
:answer4: Аутоматско наслеђивање
:correct: 3

Шта омогућавају генерички интерфејси у програмском језику C#?
```

## Питање 2

```text
interface IInterfejs<T> { }
interface<T> IInterfejs { }
generic interface IInterfejs(T) { }
interface IInterfejs where T : class
```

```{mchoice}
:answer1: Правилно је дефинисан генерички интерфејс у првој линији кода.
:answer2: Правилно је дефинисан генерички интерфејс у другој линији кода.
:answer3: Правилно је дефинисан генерички интерфејс у трећој линији кода.
:answer4: Правилно је дефинисан генерички интерфејс у четвртој линији кода.
:correct: 1

Како се дефинише генерички интерфејс?
```

## Питање 3

```{mchoice}
:answer1: Да буде апстрактна
:answer2: Да наслеђује базну класу
:answer3: Да дефинише методе интерфејса за конкретан тип или буде генеричка
:answer4: Да користи само тип object
:correct: 3

Шта мора "урадити" класа која имплементира генерички интерфејс?
```

## Питање 4

```cs
interface IPrikazivac<T>
{
    void Prikazi(T vrednost);
}

class PrikazivacString : IPrikazivac<string>
{
    public void Prikazi(string vrednost)
    {
        Console.WriteLine("String: " + vrednost);
    }
}

// ...

IPrikazivac<string> ps = new PrikazivacString();  
ps.Prikazi("Zdravo!");
```

```{mchoice}
:answer1: Исписаће се Zdravo!
:answer2: Исписаће се String: Zdravo!
:answer3: Јавиће се грешка приликом извршавања програма
:answer4: Јавиће се грешка приликом компајлирања
:correct: 2

Шта ће се десити након извршавања датог кода?
```

## Питање 5

```text
interface B<T> : interface A<T>
interface B : A<T>
interface B<T> : A<T>
interface B<T> inherits A<T>
```

```{mchoice}
:answer1: Правилна је синтакса у првој линији кода.
:answer2: Правилна је синтакса у другој линији кода.
:answer3: Правилна је синтакса у трећој линији кода.
:answer4: Правилна је синтакса у четвртој линији кода.
:correct: 3

Како изгледа исправна синтакса где се врши наслеђивање једног генеричког
интерфејса од другог?
```

## Питање 6

```{mchoice}
:answer1: ITextFormatter
:answer2: IDataBinder
:answer3: IComparable
:answer4: IPrinter
:correct: 3

Који од следећих интерфејса је унапред дефинисан у .NET-у?
```

## Питање 7

```{mchoice}
:answer1: IComparer
:answer2: IEnumerator
:answer3: IEnumerable
:answer4: IIterable
:correct: 3

Који интерфејс омогућава итерирање кроз колекције у .NET-у?
```
