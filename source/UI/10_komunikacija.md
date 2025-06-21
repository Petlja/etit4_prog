# Комуникација између форми

У апликацијама које користе више форми, често је потребно да се те форме
међусобно обавештавају или размењују податке.

Када је друга форма модална, то можеш искористити да видиш које је дугме
корисник притиснуо на њој. На пример, нека на другој форми постоје два дугмета
`OK` и `Cancel` и нека су дефинисане методе за клик на та два дугмета:

```cs
private void button1_Click(object sender, EventArgs e)
{
    this.DialogResult = DialogResult.OK;
}
 
private void button2_Click(object sender, EventArgs e)
{
    this.DialogResult = DialogResult.Cancel;
}
```

На првој форми можеш видети шта је корисник притиснуо на другој форми овако:

```cs
private void button1_Click(object sender, EventArgs e)
{
    if (f2.ShowDialog() == DialogResult.OK)
        MessageBox.Show("Korisnik je pritisnuo dugme OK");
    else
        MessageBox.Show("Korisnik je pritisnuo dugme Cancel");
}
```

Можеш приметити и да је клик на дугме `Cancel` једнак клику на дугме `Close` у
насловној линији друге форме.

Модалне форме можеш искористити и за прослеђивање вредности. На пример, једна
форма може да шаље податке другој форми, или да прочита податке које је
корисник унео у другу форму. У овој лекцији ћеш научити неколико основних
начина за комуникацију између форми у Windows Forms апликацијама.

## Слање података кроз конструктор

Једноставан и чест начин је да једна форма, приликом креирања друге форме,
пошаље податке кроз конструктор. На пример, ако је потребно послати неки текст
из `Form1` у `Form2`, кôд у другој форми може да изгледа овако...

```cs
public partial class Form2 : Form
{
    public Form2(string poruka)
    {
        InitializeComponent();
        label1.Text = poruka;
    }
}
```

...а кôд у првој форми овако:

```cs
private void btnOtvoriFormu_Click(object sender, EventArgs e)
{
    string poruka = "Pozdrav iz Form1 preko konstruktora!";
    Form2 f2 = new Form2(poruka);
    f2.ShowDialog();
}
```

## Приступ јавним пољима или својствима друге форме

Други, такође прилично једноставан начин је да дефинишеш својства у другој
форми, чиме омогућаваш другој форми да прими податке из прве. На пример, кôд у
другој форми може да изгледа овако...

```cs
public partial class Form2 : Form
{
    public string Poruka
    {
        get { return label1.Text; }
        set { label1.Text = value; }
    }

    public Form2()
    {
        InitializeComponent();
    }
}
```

...а кôд у првој форми овако:

```cs
private void btnOtvoriFormu_Click(object sender, EventArgs e)
{
    Form2 f2 = new Form2();
    f2.Poruka = "Pozdrav iz Form1 preko svojstva!";
    f2.ShowDialog();
}
```

## Читање података из друге форме

Када корисник уноси податке у другу форму, нпр. у контролу `TextBox`, прва
форма може да их прочита након што се друга форма затвори. У овом случају
најбоље је да друга форма буде модална. На пример, кôд у другој форми може да
изгледа овако...

```cs
public partial class Form2 : Form
{
    public Form2()
    {
        InitializeComponent();
    }

    private string ime;
    public string Ime
    {
        get { return textBox1.Text; }
        set { textBox1.Text = value; }
    }

    private void btnPotvrdi_Click(object sender, EventArgs e)
    {
        this.DialogResult = DialogResult.OK;
    }
}
```

...а кôд у првој форми овако:

```cs
private void btnUnesiIme_Click(object sender, EventArgs e)
{
    Form2 f2 = new Form2();
    if (f2.ShowDialog() == DialogResult.OK)
    {
        MessageBox.Show("Na drugoj formi uneto je ime: " + f2.Ime);
    }
}
```

У овом примеру корисник уноси име у контролу `textBox1` постављену на `Form2`.
Када кликне на дугме "Potvrdi", форма се затвара са резултатом `OK`. У `Form1`,
након `ShowDialog()`, чита се својство `Ime` које враћа текст који је корисник
унео у `textBox1`.

## Коришћење метода за комуникацију

Уместо својстава, форми можеш проследити податке преко посебне методе. На
пример, кôд у другој форми може да изгледа овако...

```cs
public partial class Form2 : Form
{
    public Form2()
    {
        InitializeComponent();
    }

    public void PostaviPoruku(string tekst)
    {
        label1.Text = tekst;
    }
}
```

...а кôд у првој форми овако:

```cs
private void btnOtvoriFormu_Click(object sender, EventArgs e)
{
    Form2 f2 = new Form2();
    f2.PostaviPoruku("Pozdrav iz Form1 preko metode!");
    f2.ShowDialog();
}
```

## Пренос података у супротном смеру

Понекад друга форма треба да промени нешто у првој. У таквом случају можеш да
проследиш референцу на прву форму другој форми. На пример, кôд у првој форми
може да изгледа овако...

```cs
public void PrimiPodatke(string podatak)
{
    label1.Text = podatak;
}

private void btnOtvoriFormu2_Click(object sender, EventArgs e)
{
    Form2 f2 = new Form2(this);
    f2.ShowDialog();
}
```

...а кôд у другој форми овако:

```cs
private Form1 baznaForma;

public Form2(Form1 forma)
{
    InitializeComponent();
    baznaForma = forma;
}

private void btnPosalji_Click(object sender, EventArgs e)
{
    baznaForma.PrimiPodatke(textBox1.Text);
    this.Close();
}
```

Овде друга форма користи референцу на прву форму како би позвала њен јавни
метод и променила приказане податке.

У овој лекцији научио си да постоји више начина за комуникацију између форми, а
избор зависи од конкретне потребе. Када апликација постаје сложенија, важно је
да добро организујеш ову комуникацију, тако да буде прегледна, стабилна и лако
одржива. У наредној лекцији ћеш упознати посебан начин рада са више докумената
у једној форми, односно MDI апликације.
