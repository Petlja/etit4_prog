# Квиз: цртање

## Питање 1

```{mchoice}
:answer1: DrawLine()
:answer2: DrawCurve()
:answer3: DrawLines()
:answer4: DrawBezier()
:correct: 1

Која метода класе Graphics се користи за цртање праве линије?
```

## Питање 2

```{mchoice}
:answer1: Pen
:answer2: Координате две тачке
:answer3: Brush
:answer4: StringFormat
:correct: 1,2

Који параметри су потребни за позив методе DrawLine() у њеном најједноставнијем облику?
```

## Питање 3

```{mchoice}
:answer1: Подешавањем SmoothingMode.None
:answer2: Подешавањем SmoothingMode.AntiAlias
:answer3: Коришћењем Pen са дебљином 1
:answer4: MatrixOrder.Prepend
:correct: 2

Како се постиже антиалиасинг при цртању линија?
```

## Питање 4

```{mchoice}
:answer1: DrawLine()
:answer2: DrawCurve()
:answer3: DrawLines()
:answer4: DrawBezier()
:correct: 3

Која метода се користи за цртање изломљене линије?
```

## Питање 5

```{mchoice}
:answer1: Flat
:answer2: Square
:answer3: Circle
:answer4: ArrowAnchor
:correct: 1,2,4

Које вредности из енумерације `LineCap` се могу користити за терминаторе линија?
```

## Питање 6

```{mchoice}
:answer1: DrawLine()
:answer2: DrawCurve()
:answer3: DrawLines()
:answer4: DrawBezier()
:correct: 2

Која метода се користи за цртање криве линије кроз низ тачака?
```

## Питање 7

```{mchoice}
:answer1: offset
:answer2: tension
:answer3: segments
:answer4: angle
:correct: 2

Који параметар утиче на "затегнутост" криве при позиву методе `DrawCurve()`?
```

## Питање 8

```{mchoice}
:answer1: DrawLine()
:answer2: DrawCurve()
:answer3: DrawBeziers()
:answer4: DrawBezier()
:correct: 3,4

Која метода омогућава цртање Безјеове криве?
```

## Питање 9

```{mchoice}
:answer1: Праћење MouseDown, MouseMove и MouseUp догађаја
:answer2: Чување тачака у низу или листи
:answer3: Позивање Invalidate() за освежавање форме
:answer4: Коришћење StringFormat за поравнање
:correct: 1,2,3

Шта је потребно за реализацију цртања слободном руком у Windows Forms апликацији?
```

## Питање 10

```{mchoice}
:answer1: DrawLine()
:answer2: DrawCurve()
:answer3: DrawRectangle()
:answer4: DrawBezier()
:correct: 3

Која метода се користи за цртање ивица правоугаоника?
```

## Питање 11

```{mchoice}
:answer1: Pen
:answer2: Brush
:answer3: RectangleFill
:answer4: Matrix
:correct: 2

Која класа се користи за попуњавање унутрашњости правоугаоника?
```

## Питање 12

```{mchoice}
:answer1: Прво цртање ивица, па попуњавање
:answer2: Прво попуњавање, па цртање ивица
:answer3: Зависи да ли је тамнија линија или позадина
:answer4: Распоред није битан и не утиче на изглед цртежа
:correct: 2

Који је исправан редослед цртања обојеног правоугаоника?
```

## Питање 13

```{mchoice}
:answer1: Коришћењем DrawEllipse() са различитим ширином и висином
:answer2: Коришћењем DrawEllipse() са истом ширином и висином
:answer3: Коришћењем DrawCircle() са углом од 360°
:answer4: Коришћењем DrawArc() са углом од 180°
:correct: 2

Како се црта круг у Windows Forms апликацији?
```

## Питање 14

```{mchoice}
:answer1: DrawArc()
:answer2: FillPie()
:answer3: DrawEllipse()
:answer4: FillRectangle()
:correct: 2

Која метода се користи за попуњавање кружног исечка?
```

## Питање 15

```{mchoice}
:answer1: Pen
:answer2: Координате и димензије правоугаоника
:answer3: Висина лука
:answer4: Почетни угао и угао кретања
:correct: 1,2,4

Који параметри су потребни за методу `DrawArc()`?
```

## Питање 16

```{mchoice}
:answer1: DrawText()
:answer2: DrawString()
:answer3: FillText()
:answer4: WriteString()
:correct: 2

Која метода се користи за исцртавање текста у Windows Forms апликацији?
```

## Питање 17

```{mchoice}
:answer1: Коришћењем SmoothingMode
:answer2: Коришћењем StringFormat са подешеним Alignment и LineAlignment
:answer3: Коришћењем MatrixOrder
:answer4: Коришћењем Pen
:correct: 2

Како се центрира текст унутар правоугаоника при цртању текста?
```

## Питање 18

```{mchoice}
:answer1: Ротира објекат око координатног почетка
:answer2: Скалира објекат
:answer3: Ресетује трансформације објекта
:answer4: Помера објекат по X и Y оси
:correct: 4

Шта ради метода `TranslateTransform()`?
```

## Питање 19

```{mchoice}
:answer1: ResetTransform()
:answer2: ClearTransform()
:answer3: ScaleTransform()
:answer4: RotateTransform()
:correct: 1

Која метода се користи за ресетовање свих трансформација објекта на подразумевано стање?
```

## Питање 20

```{mchoice}
:answer1: Коришћењем Крамеровог правила
:answer2: Коришћењем методе зрака
:answer3: Методом intercept()
:answer4: Методом collisionDetect()
:correct: 1

Како се израчунава тачка пресека две праве у апликацији за графичко решавање
система линеарних једначина из претходне лекције?
```
