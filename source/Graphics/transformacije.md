# Трансформације графичког објекта

Трансформације графичких објеката представљају скуп операција које мењају
положај, оријентацију и величину објекта у координатном систему. Ове операције
се најчешће користе у комбинацији са класама
[`Graphics`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics?view=netframework-4.8)
и трансформационим матрицама
[`Matrix`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.drawing2d.matrix?view=netframework-4.8).

Основне трансформације су:

- **Транслација** (енгл. *Translation*) представља померање објекта,
- **Ротација** (енгл. *Rotation*) представља ротирање објекта,
- **Скалирање** (енгл. *Scaling*) представља промену величине објекта и
- **Ресет** (енгл. *Reset*) представља враћање у почетно стање.

Основна форма за цртање може да изгледа овако:

```cs
using System;
using System.Drawing;
using System.Windows.Forms;

public class TransformExample : Form
{
    public TransformExample()
    {
        this.Text = "Графичке трансформације";
        this.Size = new Size(500, 500);
        this.Paint += new PaintEventHandler(this.OnPaint);
    }

    private void OnPaint(object sender, PaintEventArgs e)
    {
        Graphics g = e.Graphics;
        Rectangle rect = new Rectangle(100, 100, 100, 60);
        g.DrawRectangle(Pens.Black, rect);
    }
}
```

## Транслација

Транслација је графичка трансформација која **помера објекат по X и Y оси**, за
шта ћеш користити методу
[`TranslateTransform()`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.translatetransform?view=netframework-4.8)
из класе `Graphics`. Постоје два преоптерећења ове методе...

```cs
public void TranslateTransform(float dx, float dy);
public void TranslateTransform(float dx, float dy, MatrixOrder order);
```

...где се у првом најједноставнијем наводе само параметри за померање по X и по
Y оси:

```cs
g.TranslateTransform(100, 50);
g.DrawRectangle(Pens.Red, 0, 0, 80, 50);
```

Извршавањем овог кода, правоугаоник ће се померити за 100 пиксела десно и за 50
пиксела надоле.

У другом преоптерећењу, поред параметара за померање по X и по Y оси, наводи се
и `MatrixOrder` којим се да дефинише редослед у којем се трансформације
примењују на графички објекат. `MatrixOrder` вредности могу бити
`MatrixOrder.Prepend` када нова трансформација треба да се примени пре
постојећих и `MatrixOrder.Append` када нова трансформација треба да се примени
после постојећих (што је и подразумевана вредност). Редослед трансформација је
важан, јер матрице трансформација нису комутативне тј. редослед утиче на
резултат! У овом примеру ће се објекат прво ротирати па онда померати...

```cs
g.RotateTransform(45);
g.TranslateTransform(100, 0, MatrixOrder.Append);
```

...док ће се у овом објекат прво померати па онда ротирати:

```cs
g.RotateTransform(45);
g.TranslateTransform(100, 0, MatrixOrder.Prepend);
```

За једноставне задатке, `TranslateTransform(dx, dy)` без `MatrixOrder` је
довољан. За комбинацију више трансформација (нпр. скалирање са ротацијом и
транслацијом), увек треба да експлицитно наведеш `MatrixOrder` ради прецизније
контроле.

## Ротација

Ротација је трансформација која ротира графички објекат око почетка
координатног система за одређени угао у степенима, у смеру супротном од кретања
казаљке на сату. У .NET Framework-у, користи се метода
[`RotateTransform`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.rotatetransform?view=netframework-4.8)
из класе `Graphics`. Постоје два преоптерећења ове методе...

```cs
public void RotateTransform(float angle);
public void RotateTransform(float angle, MatrixOrder order);
```

...где се у првом најједноставнијем наводи само угао ротације у степенима:

```cs
g.RotateTransform(45);
g.DrawRectangle(Pens.Red, 0, 0, 100, 60);
```

Извршавањем овог кода, правоугаоник ће се ротирати за 45° око почетка
координатног система (0,0).

У другом преоптерећењу, наводи се и параметар `MatrixOrder`, који ти као и у
претходном примеру омогућује да одредиш када се ротација примењује у односу на
остале трансформације. У првом примеру...

```cs
g.RotateTransform(45);
g.TranslateTransform(100, 0, MatrixOrder.Append);
```

...прво се извршава ротација, па онда транслација, тј. прво се објекат ротира у
месту, па се тек онда помера, док се у другом примеру...

```cs
g.TranslateTransform(100, 0, MatrixOrder.Prepend);
g.RotateTransform(45);
```

...објекат се прво помера, па онда ротира око почетка координатног система.

Ротација се увек извршава око координатног почетка (0,0), па ако желиш да
ротираш објекат око његовог центра, прво га мораш преместити, као у следећем
примеру...

```cs
Rectangle r = new Rectangle(100, 100, 100, 60);
g.TranslateTransform(r.X + r.Width / 2, r.Y + r.Height / 2);
g.RotateTransform(45);
g.DrawRectangle(Pens.Blue, -r.Width / 2, -r.Height / 2, r.Width, r.Height);
```

...где је ротација извршена око центра правоугаоника.

Значи, ротација увек ротира цео координатни систем, не само објекат. Да би
ротација била око центра објекта, треба да користиш `TranslateTransform` пре
ротације.

## 3. Скалирање (Scaling)

Скалирање је трансформација која мења **величину графичког објекта** – може да
га увећа или умањи по X и/или Y оси. У .NET-у се користи метода
[`ScaleTransform`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.scaletransform?view=netframework-4.8)
из класе `Graphics`. Постоје два преоптерећења ове методе...

```cs
public void ScaleTransform(float sx, float sy);
public void ScaleTransform(float sx, float sy, MatrixOrder order);
```

...где се у првом најједноставнијем наводи само фактор скалирања по X и по Y
оси:

```cs
g.ScaleTransform(1.5f, 0.5f);
g.DrawRectangle(Pens.Red, 100, 100, 100, 60);
```

Извршавањем овог кода, нацртаће се правоугаоник који је 1.5 пута шири, а 2 пута
нижи (0.5 пута мањи по Y оси).

У другом преоптерећењу, наводи се и параметар `MatrixOrder`, који ти као и у
претходном примеру омогућује да одредиш када се скалирање примењује у односу на
друге трансформације. У првом примеру...

```cs
g.TranslateTransform(100, 100);
g.ScaleTransform(2.0f, 2.0f, MatrixOrder.Append);
```

...прво се премешта координатни систем, па се након тога све увећава 2 пута,
док се у другом примеру...

```cs
g.ScaleTransform(2.0f, 2.0f, MatrixOrder.Prepend);
g.TranslateTransform(100, 100);
```

...прво се све увећава дупло, па се онда координате (укључујући померање)
удвоструче.

Да би се објекат увећао или умањио око свог центра, потребно је да прво извршиш
транслацију и помериш координатни систем у центар објекта, па затим скалираш
објекат и на крају вратиш координатног почетак у горњи леви угао.
У следећем примеру...

```cs
Rectangle r = new Rectangle(100, 100, 80, 50);
g.TranslateTransform(r.X + r.Width / 2, r.Y + r.Height / 2);
g.ScaleTransform(1.5f, 1.5f);
g.TranslateTransform(-r.Width / 2, -r.Height / 2);
g.DrawRectangle(Pens.Blue, 0, 0, r.Width, r.Height);
```

...правоугаоник се увећава тачно око своје средине.

Вредност параметара методе `ScaleTransform` 1.0F значи без промене, вредност
већа од 1.0F увећава, а мања од 1.0F умањује. Негативна вредност обрће објекат,
на пример, -1 по X оси обрће објекат уназад. Ако желиш да скалирање утиче и на
касније трансформације, онда треба да користиш `MatrixOrder.Prepend`.

## Ресетовање трансформација

Након што применимо једну или више трансформација, координатни систем се трајно
мења унутар исте `Paint` операције. Ако не вратимо тај систем у почетно стање,
све наредне операције цртања биће измењене. За то служи метода:

```cs
public void ResetTransform();
```

Метода `ResetTransform()` ресетује све трансформације примењене на Graphics
објекат и враћа координатни систем у подразумевано стање (идентитет матрица:
без померања, ротирања или скалирања).

```cs
g.TranslateTransform(100, 50);
g.DrawRectangle(Pens.Red, 0, 0, 80, 50);
g.ResetTransform();
g.DrawRectangle(Pens.Black, 0, 0, 80, 50);
```

У овом примеру, први правоугаоник ће бити померен, а други ће остати у
координатама (0, 0).

Где и када користити ResetTransform? Између различитих трансформисаних цртежа
унутар истог OnPaint метода Пре нове трансформације, ако не желиш да се стара
примени и на нови објекат У сложеним сценама као граница између делова који
имају различите координатне системе

Graphics објекат задржава све трансформације током свог животног века унутар
Paint догађаја. Ако их не ресетујеш, ефекти ће се гомилати и утицати на све
наредне елементе.

Пример са више трансформација и ResetTransform

```cs
private void OnPaint(object sender, PaintEventArgs e)
{
    Graphics g = e.Graphics;
    Rectangle rect = new Rectangle(0, 0, 100, 60);
    g.ScaleTransform(2.0f, 2.0f);
    g.DrawRectangle(Pens.Red, rect);
    g.ResetTransform();
    g.RotateTransform(30);
    g.DrawRectangle(Pens.Blue, rect);
    g.ResetTransform();
    g.TranslateTransform(150, 100);
    g.DrawRectangle(Pens.Green, rect);
    g.ResetTransform();
    g.DrawRectangle(Pens.Black, rect);
}
```

Сваки објекат у овом примеру се црта у засебном координатном систему.

Уколико креираш сопствени координатни систем за сваки графички елемент,
обавезно користи `ResetTransform()` пре нове серије трансформација. То
повећава читљивост и предвидљивост кода.
