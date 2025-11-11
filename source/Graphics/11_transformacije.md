# Трансформације графичког објекта

Трансформације графичког објекта су скуп операција којима се мења положај,
оријентација и величина објекта у координатном систему. Ове операције су
дефинисане у класи
[`Graphics`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics?view=netframework-4.8),
а често се користе у комбинацији са трансформационим матрицама
[`Matrix`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.drawing2d.matrix?view=netframework-4.8).

**Важно**: Трансформације не мењају сам објекат, већ **координатни систем** у
коме се објекат црта. То значи да све наредне операције цртања користе тај
измењени координатни систем.

Основне трансформације су:

- **Транслација** (енгл. *Translation*) представља померање објекта,
- **Ротација** (енгл. *Rotation*) представља ротирање објекта,
- **Скалирање** (енгл. *Scaling*) представља промену величине објекта, и
- **Ресет** (енгл. *Reset*) представља враћање у почетно стање.

Нека је задат правоугаоник са позицијом (100, 50), ширином 100 и висином 60
пиксела:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
    }
}
```

![Трансформације](./images/Transformacije1.png)

## Транслација

Транслација је графичка трансформација која **помера објекат по X и Y оси**. За
реализацију транслације користи се метода
[`TranslateTransform()`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.translatetransform?view=netframework-4.8)
из класе `Graphics`. Постоје два преоптерећења ове методе:

```cs
TranslateTransform(float, float);
TranslateTransform(float, float, MatrixOrder);
```

У првом, најједноставнијем, наводе се само параметри за померање по X и по
Y оси. Вредности се аутоматски конвертују у `float` тип. У следећем примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.TranslateTransform(20, 20);
        g.DrawRectangle(p, r);
    }
}
```

правоугаоник се помера за 20 пиксела удесно и 20 пиксела надоле.

![Транслација](./images/Translacija1.png)

### MatrixOrder - редослед трансформација

У другом преоптерећењу, поред параметара за померање по X и по Y оси, наводи се
и `MatrixOrder` којим се дефинише редослед у којем се трансформације
примењују на графички објекат.

`MatrixOrder` вредности могу бити:

- **`MatrixOrder.Prepend`** - нова трансформација се примењује **пре**
постојећих
- **`MatrixOrder.Append`** - нова трансформација се примењује **после**
постојећих (подразумевано)

**Редослед трансформација је критичан**, јер матрице трансформација нису
комутативне, тј. редослед утиче на коначан резултат!

| Редослед позива | `MatrixOrder.Append` | `MatrixOrder.Prepend` |
|-----------------|----------------------|-----------------------|
| `RotateTransform(30)` → `TranslateTransform(20, 20)` | **Прво ротација**, затим транслација | **Прво транслација**, затим ротација |

У овом примеру ће се објекат прво ротирати па онда померати:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.RotateTransform(30);
        g.TranslateTransform(20, 20, MatrixOrder.Append);
        g.DrawRectangle(p, r);
    }
}
```

![Транслација](./images/Translacija2.png)

Док ће се у овом објекат прво померати па онда ротирати:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(20, 20, 120, 80);
        g.DrawRectangle(p, r);
        g.RotateTransform(30);
        g.TranslateTransform(20, 20, MatrixOrder.Prepend);
        g.DrawRectangle(p, r);
    }
}
```

![Транслација](./images/Translacija3.png)

За једноставне задатке, `TranslateTransform(float, float)` без `MatrixOrder` је
довољан. За комбинацију више трансформација (нпр. скалирање са ротацијом и
транслацијом), увек треба да експлицитно наведеш `MatrixOrder` ради прецизније
контроле.

## Ротација

Ротација је трансформација која ротира графички објекат око почетка
координатног система за одређени угао у степенима, у смеру супротном од кретања
казаљке на сату. За реализацију ротације користи се метода
[`RotateTransform`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.rotatetransform?view=netframework-4.8)
из класе `Graphics`. Постоје два преоптерећења ове методе:

```cs
RotateTransform(float);
RotateTransform(float, MatrixOrder);
```

У првом, најједноставнијем, наводи се само угао ротације у степенима. У
следећем примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.RotateTransform(30);
        g.DrawRectangle(p, r);
    }
}
```

правоугаоник ће се ротирати за 30° око почетка координатног система (0,0).

![Ротација](./images/Rotacija1.png)

У другом преоптерећењу, наводи се и параметар `MatrixOrder`, који ти, као и код
транслације, омогућава да одредиш када се ротација примењује у односу на
остале трансформације. У првом примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.RotateTransform(30);
        g.TranslateTransform(20, 20, MatrixOrder.Append);
        g.DrawRectangle(p, r);
    }
}
```

![Ротација](./images/Rotacija2.png)

прво се извршава ротација, па онда транслација, тј. прво се објекат ротира у
месту, па се тек онда помера. Док се у другом примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.RotateTransform(30);
        g.TranslateTransform(20, 20, MatrixOrder.Prepend);
        g.DrawRectangle(p, r);
    }
}
```

![Ротација](./images/Rotacija3.png)

објекат прво помера, па онда ротира око почетка координатног система.

### Ротација око центра објекта

Ротација се увек извршава око координатног почетка (0, 0), па ако желиш да
ротираш објекат око његовог центра, прво га мораш преместити. Ево како то функционише:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.TranslateTransform(r.X + r.Width / 2, r.Y + r.Height / 2);
        g.RotateTransform(30);
        g.DrawRectangle(p, -r.Width / 2, -r.Height / 2, r.Width, r.Height);
    }
}
```

Негативне координате `(-r.Width / 2, -r.Height / 2)` постављају правоугаоник
тако да његов центар буде у новом координатном почетку $(0, 0)$, који је
померен у центар оригиналног правоугаоника. На тај начин, ротација око $(0, 0)$
у новом систему одговара ротацији око центра правоугаоника у оригиналном
систему.

![Ротација](./images/Rotacija4.png)

Ротација увек ротира цео координатни систем, не само објекат. Да би ротација
била око центра објекта, треба да користиш методу `TranslateTransform` пре
ротације, ради померања центра око којег се објекат ротира!

## Скалирање

Скалирање је трансформација која мења **величину графичког објекта** – може да
га увећа или умањи по X и/или Y оси. За реализацију скалирања користи се метода
[`ScaleTransform`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.scaletransform?view=netframework-4.8)
из класе `Graphics`. Постоје два преоптерећења ове методе:

```cs
ScaleTransform(float, float);
ScaleTransform(float, float, MatrixOrder);
```

У првом, најједноставнијем, наводи се само фактор скалирања по $X$ и по $Y$
оси. У следећем примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.ScaleTransform(1.5F, 0.5F);
        g.DrawRectangle(p, r);
    }
}
```

нацртаће се правоугаоник који је 1.5 пута шири по $X$ оси, а смањен на половину
висине по $Y$ оси - фактор `0.5F` значи 50% оригиналне висине, односно два пута
нижи. Примера ради, одређени фактори скалирања на објекат утичу овако:

- **1.0F** = без промене
- **> 1.0F** = увећање (нпр. 2.0F, дупло веће)
- **< 1.0F** = умањење (нпр. 0.5F, половина величине)
- **Негативне вредности** = обртање (нпр. -1.0F по X оси обрће објекат хоризонтално)

![Скалирање](./images/Skaliranje1.png)

Скалирање утиче и на дебљину пенкале! У горњем примеру, линије скалираног
правоугаоника су дебље по $X$ оси, а тање по $Y$ оси.

У другом преоптерећењу, наводи се и параметар `MatrixOrder`, који ти, као и у
претходним примерима, омогућује да одредиш када се скалирање примењује у односу
на друге трансформације. У овом примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.TranslateTransform(50, 50);
        g.ScaleTransform(1.5F, 0.5F, MatrixOrder.Append);
        g.DrawRectangle(p, r);
    }
}
```

![Скалирање](./images/Skaliranje2.png)

прво се врши померање, па након тога скалирање. Док се у овом примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.TranslateTransform(50, 50);
        g.ScaleTransform(1.5F, 0.5F, MatrixOrder.Prepend);
        g.DrawRectangle(p, r);
    }
}
```

![Скалирање](./images/Skaliranje3.png)

прво врши скалирање, па након тога померање.

### Скалирање око центра објекта

Да би се објекат увећао или умањио око свог центра, потребно је да прво извршиш
транслацију и помериш координатни систем у центар објекта, па затим скалираш
објекат и на крају вратиш координатни почетак у горњи леви угао. У следећем
примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(100, 50, 100, 60);
        g.DrawRectangle(p, r);
        g.TranslateTransform(r.X + r.Width / 2, r.Y + r.Height / 2);
        g.ScaleTransform(1.5F, 1.5F);
        g.TranslateTransform(-r.Width / 2, -r.Height / 2);
        g.DrawRectangle(p, 0, 0, r.Width, r.Height);
    }
}
```

правоугаоник се увећава тачно око своје средине.

![Скалирање](./images/Skaliranje4.png)

## Ресетовање трансформација

Након што примениш једну или више трансформација, координатни систем се трајно
мења унутар исте `Paint` операције. Ако не вратиш тај систем у почетно стање,
све наредне операције цртања биће измењене. За враћање система у почетно стање
користи се метода
[`ResetTransform`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.resettransform?view=netframework-4.8).
Ова метода користи се без параметара и нема преоптерећења:

```cs
ResetTransform();
```

Метода `ResetTransform()` ресетује све трансформације примењене на `Graphics`
објекат и враћа координатни систем у подразумевано стање. Ово је корисно ако
желиш да црташ нови објекат без утицаја претходних трансформација. У следећем
примеру:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        g.TranslateTransform(100, 50);
        g.DrawRectangle(p, 0, 0, 80, 50);
        g.ResetTransform();
        g.DrawRectangle(p, 0, 0, 80, 50);
    }
}
```

први правоугаоник ће бити померен, док ће други бити у координатном почетку,
иако су за цртање оба правоугаоника коришћене исте наредбе.

![Ресетовање трансформација](./images/Reset1.png)

`Graphics` објекат задржава све трансформације током свог животног века
унутар `Paint` догађаја. Ако их не ресетујеш, ефекти ће се гомилати и утицати на
све наредне елементе.

У следећем примеру са више трансформација:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(0, 0, 100, 60);
        g.ScaleTransform(2.0f, 2.0f);
        g.DrawRectangle(p, r);
        g.ResetTransform();
        g.RotateTransform(30);
        g.DrawRectangle(p, r);
        g.ResetTransform();
        g.TranslateTransform(100, 100);
        g.DrawRectangle(p, r);
        g.ResetTransform();
        g.DrawRectangle(p, r);
    }
}
```

сваки објекат се црта у засебном координатном систему, захваљујући методи
`ResetTransform()`.

![Ресетовање трансформација](./images/Reset2.png)

За сложеније сценарије, уместо `ResetTransform()`, можеш користити методе
`Save()` и `Restore()` класе `Graphics`. Метода `Save()` враћа објекат типа
`GraphicsState` који чува комплетно стање `Graphics` објекта:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen p = new Pen(Color.Black, 3))
    {
        Rectangle r = new Rectangle(50, 50, 100, 60);
        GraphicsState originalnoStanje = g.Save();        
        g.RotateTransform(30);
        g.TranslateTransform(50, 50);
        g.DrawRectangle(p, r);
        g.Restore(originalnoStanje);
        g.DrawRectangle(p, r);
    }
}
```

Овај приступ је ефикаснији за сложене апликације где треба да се неколико пута
враћаш на исто стање.

## Комбиновање трансформација

Како изгледа комбинација свих трансформација у једном примеру? Креирање облика
који је скалиран, ротиран и позициониран:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen okvir = new Pen(Color.DarkBlue, 2))
    using (Brush pozadina = new SolidBrush(Color.LightBlue))
    {
        Rectangle kartica = new Rectangle(0, 0, 120, 80);
        g.FillRectangle(pozadina, kartica);
        g.DrawRectangle(okvir, kartica);
        g.DrawString("1", Font, Brushes.Black, 50, 30);
        GraphicsState stanje = g.Save();
        g.TranslateTransform(150, 0);
        g.ScaleTransform(0.8F, 0.8F);
        g.FillRectangle(pozadina, kartica);
        g.DrawRectangle(okvir, kartica);
        g.DrawString("2", Font, Brushes.Black, 50, 30);
        g.Restore(stanje);
        stanje = g.Save();
        g.TranslateTransform(60, 150);
        g.RotateTransform(30);
        g.TranslateTransform(-60, -40);
        g.FillRectangle(pozadina, kartica);
        g.DrawRectangle(okvir, kartica);
        g.DrawString("3", Font, Brushes.Black, 50, 30);
        g.Restore(stanje);
        stanje = g.Save();
        g.TranslateTransform(250, 100);
        g.RotateTransform(-15);
        g.ScaleTransform(1.2F, 1.2F);
        g.TranslateTransform(-60, -40);
        g.FillRectangle(pozadina, kartica);
        g.DrawRectangle(okvir, kartica);
        g.DrawString("4", Font, Brushes.Black, 50, 30);
        g.Restore(stanje);
    }
}
```

Овај пример демонстрира како различите комбинације трансформација могу да
створе различите визуелне ефекте.

Трансформације у графици омогућавају лако премештање, ротирање и скалирање
објеката. Кључне ствари које треба да запамтиш:

1. **Трансформације мењају координатни систем**, не сам објекат
2. **Редослед трансформација је критичан** - контролише се параметром `MatrixOrder`
3. **Ротација и скалирање се увек врше око (0, 0)** - користи транслацију да промениш центар
4. **`ResetTransform()` враћа систем у почетно стање** - користи га између независних цртања
5. За сложене сценарије, користи **`Save()`/`Restore()`** уместо `ResetTransform()`
6. **Скалирање утиче на дебљину линија** - узми то у обзир при дизајнирању

Уколико креираш сопствени координатни систем за сваки графички елемент, обавезно
користи `ResetTransform()` или `Save()`/`Restore()` пре нове серије трансформација.
То повећава читљивост и предвидљивост кода.
