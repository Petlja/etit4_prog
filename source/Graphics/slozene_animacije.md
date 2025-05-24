# Сложене анимације

У Windows Forms апликацијама можеш да пустиш машти на вољу и креираш и веома
сложене анимације. Пример може да буде путовање кроз свемир:

```cs
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Windows.Forms;

namespace PutovanjeKrozSvemir
{
    public partial class Form1 : Form
    {
        private class zvezda
        {
            public float X { get; set; }
            public float Y { get; set; }
            public float Z { get; set; }
            public float Brzina { get; set; }
        }

        private readonly List<zvezda> zvezde = new List<zvezda>();
        private readonly Random rand = new Random();
        private Timer timer;

        public Form1()
        {
            InitializeComponent();
            this.DoubleBuffered = true;
            this.Width = 640;
            this.Height = 480;
            this.Text = "Putovanje kroz svemir";
            this.BackColor = Color.Black;
            for (int i = 0; i < 500; i++)
            {
                zvezde.Add(KreirajZvezdu(true));
            }
            timer = new Timer();
            timer.Interval = 50;
            timer.Tick += Timer_Tick;
            timer.Start();
        }

        private zvezda KreirajZvezdu(bool randomZ = false)
        {
            return new zvezda
            {
                X = rand.Next(-this.Width, this.Width),
                Y = rand.Next(-this.Height, this.Height),
                Z = randomZ ? (float)rand.NextDouble() : 1,
                Brzina = 2 + (float)rand.NextDouble() * 5
            };
        }

        private void Timer_Tick(object sender, EventArgs e)
        {
            for (int i = 0; i < zvezde.Count; i++)
            {
                zvezde[i].Z -= zvezde[i].Brzina * 0.001f;
                if (zvezde[i].Z <= 0)
                {
                    zvezde[i] = KreirajZvezdu();
                }
            }
            this.Invalidate();
        }

        protected override void OnPaint(PaintEventArgs e)
        {
            base.OnPaint(e);
            var g = e.Graphics;
            g.SmoothingMode = SmoothingMode.AntiAlias;
            float centarX = this.Width / 2f;
            float centarY = this.Height / 2f;
            foreach (var zvezda in zvezde)
            {
                float zvezdaX = centarX + (zvezda.X / zvezda.Z);
                float zvezdaY = centarY + (zvezda.Y / zvezda.Z);
                float velicina = 5 * (1 - zvezda.Z);
                if (velicina < 1) velicina = 1;
                int alfa = (int)(255 * (1 - zvezda.Z));
                alfa = Math.Max(0, Math.Min(255, alfa));
                Color bojaZvezde = Color.FromArgb(alfa, 255, 255, 255);
                using (var b = new SolidBrush(bojaZvezde))
                {
                    g.FillEllipse(b, zvezdaX, zvezdaY, velicina, velicina);
                }
            }
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

![Путовање кроз свемир](./images/Svemir.png)

Овај програм симулира утисак летења кроз свемир са звездама које се
приближавају посматрачу. Да детаљније разумеш како функционише, анализирај га
део по део. Прво, креирана је класа `zvezda` која представља једну звезду у
свемиру. Свака звезда има своје координате (X, Y) и додатну координату Z која
представља дубину - колико је звезда удаљена од посматрача. Такође, свака
звезда има своју брзину кретања, што ће касније помоћи да се створим утисак
перспективе.

При покретању програма, у конструктору форме постављају се основна подешавања.
Користи се двоструко баферисање `DoubleBuffered` да би се избегло треперење при
анимацији. Форма има црну позадину и одређену величину. На почетку се креира
500 звезда које су насумично распоређене по екрану, са различитим дубинама и
брзинама.

`Тimer` компонента је "срце" анимације. Подешен је да се активира сваких 16
милисекунди. При сваком активирању тајмера, звезде се померају ка посматрачу
смањујући своју Z координату. Када звезда "прође" поред посматрача Z постане
мања или једнака 0, па се замењује новом звездом која се појављује у даљини.

Најзанимљивији део је цртање звезда. Користимо се 3D перспектива - звезде које
су ближе изгледају веће и светлије. Позиција на екрану израчунава се делећи X и
Y координате са Z вредношћу, што даје утисак дубине. Величина звезде и њена
провидност (алфа канал) зависе од удаљености - ближе звезде су веће и јасније
видљиве.

Када корисник затвори форму, важно је да се заустави тајмер и ослободе ресурси,
што се радим у методи `OnFormClosing`. Овај приступ осигурава да нема цурења
ресурса након затварања програма.

Ова анимација ефикасно користи једноставан математички приказ перспективе да би
створила убедљив утисак кретања кроз свемир. Комбинацијом случајних позиција,
брзина и правилног рачунања величина и боја звезда, добија се визуелно
привлачан ефекат уз релативно једноставан код.
