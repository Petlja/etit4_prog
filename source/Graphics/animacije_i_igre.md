# Анимације и рачунарске игрице

Нека је задатак да креираш поједностављену верзију чувене игре Понг. Лоптица
треба да се креће по екрану и одбија од горње, леве и десне ивице форме. Када
удари у рекет (праву на дну), одбија се нагоре. Играч контролише рекет
користећи тастере лево и десно на тастатури. Рекет се креће само хоризонтално
уз доњу ивицу форме. Ако лоптица прође поред рекета и додирне доњу ивицу форме,
игра треба да се заврши и прикаже порука о губитку. Перформансе игре треба да
оптимизујеш коришћењем `DoubleBuffered` својства за глатко приказивање и
тајмером који ажурира игру око 60 пута у секунди.

```cs
using System;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Windows.Forms;

namespace Grafika
{
    public partial class Form1 : Form
    {
        private int lopticaX = 100;
        private int lopticaY = 100;
        private int lopticaBrzinaX = 3;
        private int lopticaBrzinaY = 3;
        private int lopticaVelicina = 20;
        private int igracSirina = 100;
        private int igracVisina = 15;
        private int igracX;
        private int igracBzina = 10;
        private bool aktivna = true;

        public Form1()
        {
            InitializeComponent();
            this.DoubleBuffered = true;
            this.Width = 640;
            this.Height = 480;
            this.Text = "Igra Pong";
            this.BackColor = Color.Black;
            igracX = (this.ClientSize.Width - igracSirina) / 2;
            Timer timer = new Timer();
            timer.Interval = 16;
            timer.Tick += Timer_Tick;
            timer.Start();
            this.KeyDown += Pomeranje;
        }

        private void Timer_Tick(object sender, EventArgs e)
        {
            if (!aktivna) return;
            lopticaX += lopticaBrzinaX;
            lopticaY += lopticaBrzinaY;
            if (lopticaX <= 0 || lopticaX + lopticaVelicina >= this.ClientSize.Width)
            {
                lopticaBrzinaX *= -1;
            }
            if (lopticaY <= 0)
            {
                lopticaBrzinaY *= -1;
            }
            if (lopticaY + lopticaVelicina >= this.ClientSize.Height - igracVisina &&
                lopticaX + lopticaVelicina >= igracX &&
                lopticaX <= igracX + igracSirina)
            {
                lopticaBrzinaY *= -1;
            }
            if (lopticaY >= this.ClientSize.Height)
            {
                aktivna = false;
                MessageBox.Show("Izgubio si!");
                this.Close();
            }
            this.Invalidate();
        }

        private void Pomeranje(object sender, KeyEventArgs e)
        {
            if (e.KeyCode == Keys.Left && igracX > 0)
            {
                igracX -= igracBzina;
            }
            else if (e.KeyCode == Keys.Right && igracX < this.ClientSize.Width - igracSirina)
            {
                igracX += igracBzina;
            }
        }
        protected override void OnPaint(PaintEventArgs e)
        {
            base.OnPaint(e);
            Graphics g = e.Graphics;
            g.SmoothingMode = SmoothingMode.AntiAlias;
            g.FillEllipse(Brushes.White, lopticaX, lopticaY, lopticaVelicina, lopticaVelicina);
            g.FillRectangle(Brushes.Yellow, igracX, this.ClientSize.Height - igracVisina, igracSirina, igracVisina);
        }
    }
}
```

Ово је основна верзија игре коју можеш да прошириш додавањем система бодовања и
топ листе најбољих играча, додавањем повећања тежине играња убрзањем лоптице и
нивоима, додавањем више лоптица итд. Уз мало труда можеш да креираш и другог
играча на горњој ивици форме којим управља рачунар и који никада не греши.
