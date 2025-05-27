https://doc.qt.io/qt-6/qabstractspinbox.html

- Inherits：[[QWidget]]
- Inherited By：[[QSpinBox]]、[QDoubleSpinBox](https://doc.qt.io/qt-6/qdoublespinbox.html)、[QDateTimeEdit](https://doc.qt.io/qt-6/qdatetimeedit.html)

# Properties

### buttonSymbols : ButtonSymbols

该属性保存当前的按钮符号模式（[ButtonSymbols](https://doc.qt.io/qt-6/qabstractspinbox.html#ButtonSymbols-enum)）。默认值为`UpDownArrows`。

请注意，某些样式可能会以相同方式呈现`PlusMinus`和`UpDownArrows`。

| 访问函数                              |                                                        |
| --------------------------------- | ------------------------------------------------------ |
| `QAbstractSpinBox::ButtonSymbols` | `buttonSymbols() const`                                |
| `void`                            | `setButtonSymbols(QAbstractSpinBox::ButtonSymbols bs)` |

### frame : bool

该属性保存 spin box 绘制自身时是否带有边框。默认为`true`。

| 访问函数   |                    |
| ------ | ------------------ |
| `bool` | `hasFrame() const` |
| `void` | `setFrame(bool)`   |






