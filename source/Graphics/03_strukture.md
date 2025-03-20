# Структура именског простора System.Drawing

Као што је у првој лекцији овог поглавља напоменутно, именски простор
`System.Drawing` у *.NET*-у садржи класе и структуре које се користе за рад са
графиком, сликама и геометријским облицима. Структуре у овом именском простору
су структуре `Color`, `Point`, `PointF`, `Size`, `SizeF`, `Rectangle` и
`RectangleF`. Ове структуре су непромењиве (енгл. `immutable`), што значи да се
њихове вредности не могу мењати након креирања.

Структура `Color` представља боју у простору ARGB (Alpha, Red, Green, Blue).
Користи се за дефинисање боја за цртање, попуњавање и друге графичке операције.
Садржи и унапред дефинисане боје (на пример `Color.Red`, `Color.Blue`) и
омогућава креирање прилагођених боја. Списак свих боја приказан је на слици
испод:

![Табела боја](./images/color-table.png)

Боја сваког пиксела презентује се 32-битним бројем, где се користи по осам
битова за алфа, црвену, зелену и плаву компоменту. Алфа компоментом дефинише се
транспарентност - 0 потпуно провидно, а 255 потпуно непровидно. Боје које се
не налазе на списку боја можеш креирати `FromArgb()` методом. На пример, црвену
боју можеш креирати на следећи начин:

```cs
Color color = Color.FromArgb(255, 0, 0);
```

Структура `Point` представља тачку у 2D простору са целобројним координатама
$(X,Y)$. Користи се за позиционирање објеката или дефинисање координата.
Структура `PointF` слична је структури `Point`, али координате су типа `float`.
Користи се за прецизније позиционирање у простору са покретним зарезом.

Структура `Size` представља димензије (ширину и висину) са целобројним
вредностима и користи се за дефинисање величина објеката или простора. Слично,
структура `SizeF` користи вредности типа `float` и користи се за прецизније
дефинисање величина са покретним зарезом.

Структура `Rectangle` представља правоугаони регион са целобројним координатама
$(X,Y)$ и димензијама `(Width, Height)`. Користи се за дефинисање области на
екрану или за рад са деловима слике. Структура `RectangleF`, слична структури
`Rectangle`, коористи `float` за координате и димензије за прецизније
дефинисање правоугаоних региона.

У следећем примеру...

```cs
protected override void OnPaint(PaintEventArgs e)
{
    this.Size = new Size(320, 240);
    this.Text = "Upotreba struktura System.Drawing";
    Graphics g = e.Graphics;
    Color prvBoja = Color.Blue;
    Color eliBoja = Color.Red;
    Color linBoja = Color.Green;
    Color txtBoja = Color.Black;
    Pen prvPen = new Pen(prvBoja, 3);
    Pen eliPen = new Pen(eliBoja, 2);
    Pen linPen = new Pen(linBoja, 2);
    Brush txtCetka = new SolidBrush(txtBoja);
    Point pocTacka = new Point(50, 50);
    Size prvVel = new Size(200, 100);
    Rectangle prv = new Rectangle(pocTacka, prvVel);
    g.DrawRectangle(prvPen, prv);
    g.DrawEllipse(eliPen, prv);
    g.DrawLine(linPen, prv.Left, prv.Top, prv.Right, prv.Bottom);
    string text = "Upotreba struktura";
    Font font = new Font("Arial", 12);
    g.DrawString(text, font, txtCetka, new PointF(prv.Left + 26.7f, prv.Top + 40.12f));
    prvPen.Dispose();
    eliPen.Dispose();
    linPen.Dispose();
    txtCetka.Dispose();
    font.Dispose();
}
```

...структуре из именског простора System.Drawing користе се за дефинисање
геометријских облика, боја, позиција и димензија. Оне омогућавају цртање
правоугаоника, елипсе, линије и текста на форми:

![Употреба структура](./images/UpotrebaStruktura.png)
