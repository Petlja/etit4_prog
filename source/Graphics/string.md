# Цртање текста

До сада си научио да се за цртање било ког графичког садржаја у Windows Forms
апликацијама користи неки објекат класе `Graphics`. Тако је омогућено да се
"нацрта" и текст методом
[`DrawString()`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.graphics.drawstring?view=netframework-4.8).
Једноставно речено, метода `DrawString()` црта текст задат у стрингу на задатој
локацији са четком и фонтом.

```cs
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    Graphics g = e.Graphics;
    g.SmoothingMode = SmoothingMode.AntiAlias;
    using (Brush cetka = new SolidBrush(Color.Black))
    {
        Font f = new Font("Arial Narrow", 32, FontStyle.Bold);
        g.DrawString("Hello, World!", f, cetka, 10.0F, 10.0F);
    }
}
```
