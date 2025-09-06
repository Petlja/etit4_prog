# Цртање елипсе и круга

У компјутерској графици, елипса и круг се цртају као облици уписани у
правоугаоник, односно унутар правоугаоника чије координате и димензије дефинишу
њихову позицију и величину. Класа `Graphics` у .NET Framework-у омогућава
једноставно цртање ових облика методом
[`DrawEllipse()`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.drawellipse?view=netframework-4.8)

## Цртање елипсе

Ова метода подразумева навођење оловке којом се црта облик, координата горњег
левог угла правоугаоника у који је уписана елипса и ширина и висина
правоугаоника. У следећем примеру...

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen olovka = new Pen(Color.Black, 3))
    {
        g.DrawEllipse(olovka, 50, 50, 150, 100);
    }
}
```

...оловком црне боје дебљине 3 пиксела, елипса је уписана у правоугаоник чији
горњи леви угао има координате $(50, 50)$, а чија ширина је 150 и висина 100
пиксела.

Метода `DrawEllipse()` има и преоптерећење где се координате и ширина и висина
наводе као реални бројеви...

```cs
g.DrawEllipse(olovka, 50.0F, 50.0F, 150.0F, 100.0F);
```

...или где се, уместо координата, ширине и висине, наводи објекат класе
`Rectangle`...

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen olovka = new Pen(Color.Black, 3))
    {
        Rectangle pravougaonik = new Rectangle(50, 50, 150, 100);
        g.DrawEllipse(olovka, pravougaonik);
    }
}
```

...или `RectangleF`:

```cs
RectangleF pravougaonik = new RectangleF(50.0F, 50.0F, 150.0F, 100.0F);
```

Значи, уместо појединачног навођења координата, ширине и висине, можеш
користити и објекте класа `Rectangle` или `RectangleF`, који инкапсулирају те
податке.

## Бојење елипсе

Ако желиш да попуниш унутрашњост елипсе, користи класу `Brush` и методу
[`FillEllipse()`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.fillellipse?view=netframework-4.8):

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Brush cetka = new SolidBrush(Color.Yellow))
    {
        g.FillEllipse(cetka, 50, 50, 150, 150);
    }
    using (Pen olovka = new Pen(Color.Black, 3))
    {
        g.DrawEllipse(olovka, 50, 50, 150, 150);
    }
}
```

Већина програмера прво попуњава облик, па тек онда црта ивицу као у овом
примеру. Ако прво нацрташ облик, попуњавање облика може да прекрије ивицу
облика.

Метода `FillEllipse()` има иста преоптерећења као и метода `DrawEllipse()`, па
можеш да их користиш паралелно у свакој ситуацији.

## Цртање круга

Да би нацртао круг, уместо правоугаоника потребно је да наведеш квадрат – како
су ширина и висина квадрата једнаки, добија се савршен круг. На пример:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Pen olovka = new Pen(Color.Black, 3))
    {
        g.DrawEllipse(olovka, 50, 50, 150, 150);
    }
}
```

Цртање и бојење елипси и кругова у Windows Forms апликацијама је једноставно и
интуитивно. Комбинујући координате, величине и боје, могуће је креирати
разноврсне и визуелно атрактивне графичке елементе.
