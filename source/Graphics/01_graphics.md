# Класа за рад са графиком

Класа `Graphics` је део **GDI+** (енгл. *Graphics Device Interface Plus*) система
програмског језика C# и налази се у именском простору
[`System.Drawing`](https://learn.microsoft.com/en-us/dotnet/api/system.drawing?view=netframework-4.8.1).
Омогућава цртање различитих графичких елемената као што су линије, облици
слике и текст, на различитим површинама (нпр. у прозору, контроли или слици).

Постоји више начина за креирање графичких објекта. Први и препоручени начин је
`PaintEventArgs` у `OnPaint` методи. Ово је најбољи приступ за цртање у
*Windows Forms* апликацијама, јер осигурава да се графика исправно освежава.
На пример, ако је потребно да нацрташ црну линију од координате $(0,0)$ до
координате $(100,100)$:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    Graphics g = e.Graphics;
    g.DrawLine(Pens.Black, 0, 0, 100, 100);
}
```

![PaintEventArgs](./images/PaintEventArgs.png)

Мање ефикасан начин за креирање графичких објекта је помоћу `CreateGraphics()`
методе која се користи када је потребно динамичко цртање, али тај начин није
препоручен јер не гарантује да ће се графика задржати након освежавања прозора:

```cs
Graphics g = this.CreateGraphics();
g.DrawLine(Pens.Blue, 0, 0, 100, 100);
```

Ако треба да црташ на слици - битмапи, можеш користити класу `Bitmap` и
креирати графички објекат из ње. На пример, ако желиш да креираш слику
димензија 800х600, на којој је нацртана црвена линија од координате $(0,0)$ до
координате $(100,100)$:

```cs
Bitmap bmp = new Bitmap(640, 480);
Graphics g = Graphics.FromImage(bmp);
g.DrawLine(Pens.Red, 0, 0, 100, 100);
bmp.Save("line.png");
```

![GraphicsFromImage](./images/GraphicsFromImage.png)

Неке од основним метода класе `Graphics` обухватају цртање линија,
правоугаоуника, елипса, текста, слика и др. На пример, нека је задатак да на
форми нацрташ једноставну сцену на којој се види пун месец и кућица, на пример
овако...

![GraphicsFromImage](./images/Kucica.png)

...где програм може да изгледа овако:

```cs
protected override void OnPaint(PaintEventArgs e)
{
    this.Text = "Crtanje kućice";
    this.Size = new Size(400, 400);
    Graphics g = e.Graphics;
    g.FillEllipse(Brushes.Yellow, 30, 30, 70, 70);
    g.DrawEllipse(Pens.Black, 30, 30, 70, 70);
    g.FillRectangle(Brushes.DarkKhaki, 100, 150, 200, 150);
    g.DrawRectangle(Pens.Black, 100, 150, 200, 150);
    Point[] krov = { new Point(100, 150), new Point(200, 50), new Point(300, 150) };
    g.FillPolygon(Brushes.DarkRed, krov);
    g.DrawPolygon(Pens.Black, krov);
    g.FillRectangle(Brushes.Blue, 170, 220, 50, 80);
    g.DrawRectangle(Pens.Black, 170, 220, 50, 80);
    g.FillRectangle(Brushes.Green, 120, 170, 40, 40);
    g.DrawRectangle(Pens.Black, 120, 170, 40, 40);
    g.FillRectangle(Brushes.Green, 240, 170, 40, 40);
    g.DrawRectangle(Pens.Black, 240, 170, 40, 40);
}
```

У прве две линије мењају се својства форме - `this.Text` поставља наслов на
*Crtanje kućice*, а *this.Size* величину прозора на 400x400 пиксела...
`e.Graphics` је објекат који омогућава цртање на форми, који се добија из
аргумента `PaintEventArgs e`, који *Windows Forms* прослеђује када је потребно
да се прозор освежи. `FillEllipse` црта пун месец жуте боје на координатама
$(30,30)$, величине 70x70 пиксела. `DrawEllipse` црта контуру тог истог круга
црном бојом. Слично, `FillRectangle` и `DrawRectangle` попуњава тело кућице
зелено-смеђом бојом и црта ивицу тела кућице црном бојом. Потом се дефинише низ
`Point[]` који садржи три тачке крова, попуњава се тамно-црвеном бојом са црном
ивицом. Потом се цртају врата као правоугаоник плаве боје са црном ивицом. Два
прозора су зелени правоугаоници са црним ивицама. Све је нацртано унутар
`OnPaint` метода, што значи да ће се поново исцртати сваки пут када се прозор
освежи.

Више о методама и својствима класе `Graphics` биће обрађено у наредним
лекцијама.
