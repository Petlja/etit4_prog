# Рад са фонтовима

У претходној лекцији научио си да користиш `FontDialog` који омогућава
кориснику да изабере фонт. У овој лекцији научићеш нешто више о раду са
фонтовима и класи
[Font](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.font?view=netframework-4.8)
из именског простора `System.Drawing`.

Класа `Font` представља фонт који се користи за приказ текста у контролама као
што су `Label`, `Button`, `TextBox` итд. Помоћу ове класе можеш да дефинишеш
сва својства фонта, без употребе дијалога.

Објекат класе `Font` садржи информације као што су: назив фонта (нпр. "Arial",
"Times New Roman"...), величина фонта у тачкама, стил фонта (нпр. Bold, Italic,
Underline...) и др.

Класа `Font` је неизмењива (имутабилна), што значи да не можеш мењати постојећи
фонт – мораш направити нови објекат са новим својствима и доделити га контроли.
Дефинисање фонта врши се на следећи начин:

```cs
Font noviFont = new Font("Arial", 14, FontStyle.Bold);
```

Овим је креиран објекат `noviFont` класе `Font`, где је назив фонта `Arial`,
величина фонта `14` тачака и стил фонта `Bold` (задебљан). Овако дефинисан фонт
може се применити на контроле на форми, на пример на лабелу:

```cs
TextLabel.Font = noviFont;
```

Класа `Font` имплементира `IDisposable` интерфејс. У овом конкретном примеру,
где се нови фонт одмах додељује `Font` својству контроле, Windows Forms систем
ће се углавном побринути за управљање ресурсима старог фонта. Међутим, ако би
креирао много `Font` објеката у петљи за неке привремене сврхе (нпр. цртање), а
не би их додељивао контролама, онда би требао да експлицитно позовеш методу
`Dispose()` на њима, или да користиш `using` блок.

Нека је креирана форма са три дугмета B, I, U, једном контролном NumericUpDown
и једном лабелом. Задатак је да се кликом на дугме B, текст у лабели се задебља,
кликом на I текст у лабели укоси, кликом на U текст у лабели подвуче и променом
вредности у NumericUpDown контроли, текст у лабели се повећава или смањује.
Ако корисник поново кликне на B, текст више не треба да буде задебљан, ако
поново кликне на I не треба више да буде укошен и ако поново кликне на U не
треба више да буде подвучен.

![Форма са контролама](./images/fontovi.png)

Решење овог задатка може да изгледа овако:

```cs
using System;
using System.Drawing;
using System.Windows.Forms;

namespace Fontovi
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            FontSizeNumUpDown.Value = (decimal)TextLabel.Font.Size;
            FontSizeNumUpDown.Minimum = 1;
            FontSizeNumUpDown.Maximum = 72;
        }

        private void BoldBtn_Click(object sender, EventArgs e)
        {
            Font trenutni = TextLabel.Font;
            FontStyle noviStil = trenutni.Style ^ FontStyle.Bold;
            TextLabel.Font = new Font(trenutni, noviStil);
        }

        private void ItalicBtn_Click(object sender, EventArgs e)
        {
            Font trenutni = TextLabel.Font;
            FontStyle noviStil = trenutni.Style ^ FontStyle.Italic;
            TextLabel.Font = new Font(trenutni, noviStil);
        }

        private void UnderlineBtn_Click(object sender, EventArgs e)
        {
            Font trenutni = TextLabel.Font;
            FontStyle noviStil = trenutni.Style ^ FontStyle.Underline;
            TextLabel.Font = new Font(trenutni, noviStil);
        }

        private void FontSizeNumUpDown_ValueChanged(object sender, EventArgs e)
        {
            Font trenutni = TextLabel.Font;
            float velicina = (float)FontSizeNumUpDown.Value;
            TextLabel.Font = new Font(trenutni.FontFamily, velicina, trenutni.Style);
        }
    }
}
```

У методама `BoldBtn_Click()`, `ItalicBtn_Click` и `UnderlineBtn_Click` за
промену стилова коришћен је оператор ексклузивне дисјункције `^`, тј. оператор
XOR. То је могуће јер је `FontStyle` набројиви тип (`enum`) обележен `[Flags]`
атрибутом, што значи да су његове вредности дизајниране да се комбинују као
флегови. Значи, операција XOR на једноставан начин додаје стил ако није
присутан, или га уклања ако већ постоји. Овај приступ омогућава и лако
комбиновање више стилова, на пример, текст може бити истовремено и задебљан
и укошен.

У решењу задатка у прве три методе коришћен је конструктор
`new Font(Font prototypeFont, FontStyle newStyle)`, а у последњој
`new Font(FontFamily family, float emSize, FontStyle style)`. Први контруктор
креира нови фонт на основу постојећег (`prototypeFont`), али са новодефинисаним
стилом (`newStyle`). Ово је практично јер аутоматски задржава и величину и
породицу фонта оригиналног објекта. Други конструктор креира нови фонт
специфициране породице (`family`), величине (`emSize`) и стила (`style`), где
`trenutni.FontFamily` обезбеђује да се задржи оригинална породица фонта. Списак
свих конструктора класе `Font` можеш пронаћи у
[званичној документацији](https://learn.microsoft.com/en-us/dotnet/api/system.drawing.font.-ctor?view=netframework-4.8).
