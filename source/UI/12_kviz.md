# Квиз: рад са више форми

## Питање 1

```cs
private void btnPrikazi_Click(object sender, EventArgs e)
{
    Form2 novaForma = new Form2();
    novaForma.Show();
}
```

```{mchoice}
:answer1: Приказује се увек иста форма
:answer2: Креира се више независних инстанци Form2
:answer3: Приказује се грешка
:answer4: Form2 се појављује у фокусу
:correct: 2

Шта се дешава када корисник више пута кликне на дугме које креира нову инстанцу форме као у датом коду?
```

## Питање 2

```{mchoice}
:answer1: if (novaForma == null || novaForma.IsDisposed)
:answer2: if (novaForma == null || novaForma.Closed)
:answer3: if (novaForma != null && novaForma.Visible)
:answer4: if (novaForma.IsDisposed == false)
:correct: 1

Које од наведених представља правилан приступ за контролу инстанци форме?
```

## Питање 3

```{mchoice}
:answer1: Show() приказује модалну форму, ShowDialog() немодалну
:answer2: Show() приказује немодалну форму, ShowDialog() модалну
:answer3: Нема разлике, обе раде исто
:answer4: ShowDialog() не може да се затвори
:correct: 2

Која је разлика између `Show()` и `ShowDialog()` метода?
```

## Питање 4

```{mchoice}
:answer1: Кроз конструктор друге форме
:answer2: Кроз јавна својства друге форме
:answer3: Кроз методе друге форме
:answer4: Кроз прослеђивање референце на прву форму
:correct: 1,2,3,4

Који од наведених начина се може користити за слање података између форми?
```

## Питање 5

```{mchoice}
:answer1: bool вредност
:answer2: DialogResult вредност
:answer3: int вредност
:answer4: string вредност
:correct: 2

Шта враћа `ShowDialog()` метода?
```

## Питање 6

```{mchoice}
:answer1: this.IsMdiContainer = true;
:answer2: this.MdiParent = true;
:answer3: this.MdiContainer = true;
:answer4: this.IsMdiParent = true;
:correct: 1

Како се форма дефинише као MDI контејнер?
```

## Питање 7

```{mchoice}
:answer1: childForm.MdiParent = this;
:answer2: childForm.IsMdiChild = true;
:answer3: childForm.MdiContainer = false;
:answer4: childForm.Parent = this;
:correct: 1

Која својства треба поставити да би форма постала MDI дете?
```

## Питање 8

```{mchoice}
:answer1: MdiLayout.Cascade
:answer2: MdiLayout.TileHorizontal
:answer3: MdiLayout.TileVertical
:answer4: MdiLayout.ArrangeIcons
:correct: 1,2,3,4

Које су доступне опције за распоређивање MDI дечијих форми?
```

## Питање 9

```{mchoice}
:answer1: this.ActiveChild
:answer2: this.ActiveMdiChild
:answer3: this.CurrentMdiChild
:answer4: this.FocusedChild
:correct: 2

Како се приступа тренутно активној MDI дечијој форми?
```

## Питање 10

```{mchoice}
:answer1: Проверити да ли је ActiveMdiChild различито од null
:answer2: Проверити да ли је ActiveMdiChild видљиво
:answer3: Кастовати ActiveMdiChild на одговарајући тип форме
:answer4: Проверити да ли је ActiveMdiChild максимизовано
:correct: 1,3

Шта треба урадити пре приступа активној MDI дечијој форми?
```
