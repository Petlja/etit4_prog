# Контрола уноса текстуалних података

У Windows Forms апликацијама корисници најчешће уносе податке преко контроле
`TextBox`. Да би се обезбедила исправност унетих података, потребно је
контролисати шта је могуће унети и проверити да ли унети подаци одговарају
очекиваном формату.

Најједноставнији начин провере је провера вредности у тренутку обраде, нпр.
када се притисне дугме...

```cs
if (textBox1.Text == "")
{
    MessageBox.Show("Polje ne sme biti prazno!");
}
```

...или:

```cs
if (!textBox1.Text.All(char.IsDigit))
{
    MessageBox.Show("Polje sme da sadrži samo cifre!");
}
```

Уколико желиш да апликација реагује чим се унос промени, користи се догађај
`TextChanged`. На пример:

```cs
private void textBox1_TextChanged(object sender, EventArgs e)
{
    if (textBox1.Text.Length > 10)
    {
        MessageBox.Show("Prekoračen je maksimalni broj karaktera!");
    }
}
```

За претходни пример, максимални број карактера можеш једноставно ограничити
својством `MaxLength` контроле `TextBox`...

```cs
textBox1.MaxLength = 10;
```

...чиме значајно поједностављујеш логику решења.

Најпрецизнији начин контроле уноса јесте ограничавање могућности уноса током
куцања. То се постиже обрадом догађаја `KeyPress`. На пример, ако желиш да
дозволиш кориснику да уноси само цифре:

```cs
private void textBox1_KeyPress(object sender, KeyPressEventArgs e)
{
    if (!char.IsControl(e.KeyChar) && !char.IsDigit(e.KeyChar))
    {
        e.Handled = true;
    }
}
```

За сложеније шаблоне (нпр. формат броја телефона, емаил адреса...), можеш да
користиш регуларне изразе које си учио у претходним лекцијама.

Добра пракса је да кориснику, поред повратне информације у контроли
`MessageBox` или лабели, прикажеж и визуелно. На пример:

```cs
if (textBox1.Text == "")
{
    textBox1.BackColor = Color.LightCoral;
    MessageBox.Show("Polje ne sme biti prazno!");
}
else
{
    textBox1.BackColor = SystemColors.Window;
}
```

Добра је пракса и да проверу уноса вредности у више контрола извршиш на једном
месту, односно у једној функцији. На пример:

```cs
private bool ValidirajUnos()
{
    if (textBoxIme.Text == "" || textBoxGodine.Text == "")
        return false;

    if (!int.TryParse(textBoxGodine.Text, out int godine))
        return false;

    return godine >= 0 && godine <= 120;
}
```

Контрола уноса текстуалних података је кључна за обезбеђивање тачности и
доследности унетих информација. У овој лекцији научио си да примениш основне
технике контроле уноса. У пракси, ове технике се често комбинују ради постизања
најбољег корисничког искуства.

У наредној лекцији биће представљена контрола уноса на нивоу поља за унос
података, која подразумева унапред дефинисана правила на нивоу UI контрола као
што су `MaskedTextBox`, `DateTimePicker` и друге.
