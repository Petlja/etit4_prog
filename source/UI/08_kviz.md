# Квиз: менији

## Питање 1

```{mchoice}
:answer1: MainMenu
:answer2: ToolStrip
:answer3: MenuStrip
:answer4: ContextMenuStrip
:correct: 3

Која контрола се користи за креирање главног менија апликације који се налази на врху форме?
```

## Питање 2

```{mchoice}
:answer1: Подешавањем својства AccessKey на F.
:answer2: Уношењем текста ALT+F: Fajl у својство Text.
:answer3: Уношењем текста &Fajl у својство Text.
:answer4: Подешавањем својства ShortcutKeys на Keys.Alt | Keys.F.
:correct: 3

Како се дефинише тастер за приступ (*access key*) за ставку менија, тако да се
мени "Fajl" може отворити комбинацијом тастера ALT+F?
```

## Питање 3

```{mchoice}
:answer1: fajlMeni.DropDownItems.Add("-");
:answer2: fajlMeni.DropDownItems.Add(new ToolStripSeparator());
:answer3: fajlMeni.DropDownItems.AddSeparator();
:answer4: Додавањем ставке и подешавањем њеног својства IsSeparator на true.
:correct: 2

Који су исправни начини за додавање сепаратора између ставки у менију креираном
у коду?
```

## Питање 4

```{mchoice}
:answer1: Постављањем својства ContextMenu на TextBox контроли.
:answer2: Позивањем методе textBox1.ShowContextMenu(kontekstniMeni);
:answer3: Постављањем својства ContextMenuStrip на TextBox контроли.
:answer4: Додавањем TextBox контроле у Items колекцију ContextMenuStrip контроле.
:correct: 3

На који начин се `ContextMenuStrip` контрола повезује са `TextBox` контролом
да би се приказала на десни клик?
```

## Питање 5

```{mchoice}
:answer1: Click
:answer2: Opening
:answer3: VisibleChanged
:answer4: Load
:correct: 2

Који догађај `ContextMenuStrip` контроле је најидеалнији за проверу стања
апликације (нпр. да ли на клипборду има текста) непосредно пре него што се мени
прикаже, како би се омогућиле или онемогућиле одређене ставке?
```

## Питање 6

```{mchoice}
:answer1: ToolStripButton
:answer2: ToolStripSeparator
:answer3: ToolStripPanel
:answer4: ToolStripComboBox
:correct: 3

Која ставка се не може директно додати у `ToolStrip` контролу преко дизајнера?
```

## Питање 7

```{mchoice}
:answer1: Постављањем својства dugmeSnimi.ShortcutKeys = Keys.Control | Keys.S;
:answer2: Постављањем својства форме KeyPreview на true и обрадом KeyDown догађаја форме.
:answer3: Умотавањем ToolStrip контроле у ToolStripContainer који аутоматски мапира пречице.
:answer4: ToolStripButton не може имати пречице са тастатуре.
:correct: 2

Како се у коду типично имплементира пречица са тастатуре (нпр. Ctrl+S) за `ToolStripButton` дугме?
```

## Питање 8

```{mchoice}
:answer1: Оба менија ће се појавити један испод другог.
:answer2: Приказаће се само нови, прилагођени контекстни мени.
:answer3: Приказаће се подразумевани мени, а прилагођени ће бити игнорисан.
:answer4: Доћи ће до грешке приликом покретања програма.
:correct: 2

Шта се дешава ако TextBox контроли, која има подразумевани контекстни мени
(Cut, Copy, Paste), доделите сопствени `ContextMenuStrip`?
```

## Питање 9

```{mchoice}
:answer1: Написати две одвојене методе за обраду Click догађаја, једну за мени и једну за дугме.
:answer2: Повезати Click догађај обе контроле на исту методу (event handler).
:answer3: У методи за дугме програмски позвати Click догађај ставке менија.
:answer4: Користити ToolStripContainer да аутоматски синхронизује акције.
:correct: 2

Шта је најбоља пракса уколико и ставка у `MenuStrip` менију ("Sačuvaj") и дугме
на `ToolStrip` траци (икона дискете) треба да изврше исту акцију?
```

## Питање 10

```{mchoice}
:answer1: Icon
:answer2: Picture
:answer3: ImageSource
:answer4: Image
:correct: 4

Које својство `ToolStripMenuItem` или `ToolStripButton` контроле се користи за
додавање иконе?
```
