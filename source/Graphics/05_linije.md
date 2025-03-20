# Цртање линија

Када се ради са графиком у програмском језику C#, један од основних елемената
за цртање су линије. Оне могу бити праве или изломљене и могу имати различите
завршетке, тзв. терминаторе линија. Фокус у овој лекцији биће на коришћењу
класе `Graphics` за цртање ових облика.

## Права линија

За цртање праве линије користи се метода `DrawLine` класе `Graphics`. Ова
метода захтева оловку (`Pen`) и две тачке које представљају почетну и крајњу
координату линије.

```cs
protected override void OnPaint(PaintEventArgs e)
{
    this.Size = new Size(320, 240);
    this.Text = "Prava linija";
    Graphics g = e.Graphics;
    using (Pen olovka = new Pen(Color.Black, 2))
    {
        g.DrawLine(olovka, 50, 100, 250, 100);
    }
}
```

![Права линија](./images/PravaLinija.png)

## Изломљена линија

Изломљена линија представља низ повезаних линијских сегмената. За њено цртање
користи се метода `DrawLines`, којој се као аргумент прослеђује низ тачака
`Point[]`. Низ `Point[]` дефинише тачке кроз које пролази изломљена линија, а
`DrawLines` повезује све тачке секвенцијално линијама.

```cs
protected override void OnPaint(PaintEventArgs e)
{
    this.Size = new Size(320, 240);
    this.Text = "Izlomljena linija";
    Graphics g = e.Graphics;
    using (Pen olovka = new Pen(Color.Black, 2))
    {
        Point[] tacke = {
            new Point(50, 50),
            new Point(100, 150),
            new Point(150, 50),
            new Point(200, 150),
            new Point(250, 50)
        };
        g.DrawLines(olovka, tacke);
    }
}
```

![Изломљена линија](./images/IzlomljenaLinija.png)

## Терминатори линијa

Терминатори линије одређују како ће се крајеви линија приказати. Подешавају се
својством `LineCap` објекта `Pen`, а неке од вредности су:

* `LineCap.Flat`, без завршног украса,
* `LineCap.Round`, заобљени крајеви линија,
* `LineCap.Square`, квадратни завршетак,
* `LineCap.ArrowAnchor`, стрелица на крају линије.
