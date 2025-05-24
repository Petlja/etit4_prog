# Анимације

У претходној лекцији упознао си се са нитима. Међутим, анимације можеш креирати
и користећи контролу `Timer` уместо тога. `Timer` је једноставнији за анимације
у Windows Forms апликацијама јер ради у главној (UI) нити и аутоматски рукује
*thread-safe* ажурирањима. Због тога није потребно ручно користити методе
`Invoke()` и бринути се о синхронизацији.

Основни кораци за креирање анимације са контролом `Timer` подразумевали би
дефинисање позиције објекта, подешавање интервала `Timer` контроле, обраду
`Tick` догађаја где се мења стање анимације, позив `Invalidate()` да би се
форма поново исцртала, и на крају преуређивање цртања у `OnPaint`.

Следећи пример показује како се исти резултат анимације црвеног круга може
постићи употребом `Timer` контроле:

```cs
using System;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Windows.Forms;

namespace AnimacijaPomocuTimera
{
    public partial class Form1 : Form
    {
        private int x = 0;
        private Timer timer;

        public Form1()
        {
            this.DoubleBuffered = true;
            this.Width = 320;
            this.Height = 240;
            this.Text = "Animacija pomoću Timer-a";
            timer = new Timer();
            timer.Interval = 50;
            timer.Tick += Timer_Tick;
            timer.Start();
        }

        private void Timer_Tick(object sender, EventArgs e)
        {
            x += 5;
            if (x > this.Width)
            {
                x = 0;
            }
            this.Invalidate();
        }

        protected override void OnPaint(PaintEventArgs e)
        {
            base.OnPaint(e);
            Graphics g = e.Graphics;
            g.SmoothingMode = SmoothingMode.AntiAlias;
            g.FillEllipse(Brushes.Red, x, 60, 40, 40);
        }

        protected override void OnFormClosing(FormClosingEventArgs e)
        {
            base.OnFormClosing(e);
            timer.Stop();
            timer.Dispose();
        }
    }
}
```

Контрола `Timer` покреће догађај `Tick` у интервалима од 50 милисекунди унутар
главне нити, па су сви позиви безбедни за рад са UI контролама. Због тога није
потребно ручно руковати нитима или синхронизацијом. Употреба `DoubleBuffered`
спречава треперење анимације, што обезбеђује глатко цртање.

Овај приступ је најприкладнији за једноставне анимације у Windows Forms
апликацијама. За сложеније задатке, који захтевају више ресурса и позадински
рад, ипак је неопходно користити нити или асинхрони рад.
