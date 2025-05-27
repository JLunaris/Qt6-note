https://doc.qt.io/qt-6/qspinbox.html

`QSpinBox`类提供 spin box 控件。

- Inherits：[[QAbstractSpinBox]]

# Properties

### value : int

该属性保存 spin box 的值。

==调用`setValue()`时，如果新值不同于旧值，会发出`valueChanged()`信号==。

value 属性还有第二个通知信号，包含 spin box 的前缀和后缀。

| 访问函数   |                     |
| ------ | ------------------- |
| `int`  | `value() const`     |
| `void` | `setValue(int val)` |

| 通知信号   |                       |
| ------ | --------------------- |
| `void` | `valueChanged(int i)` |

### prefix : QString

该属性保存 spin box 的**前缀**。默认值为空字符串。

前缀会被预置到显示值的开头，通常用于显示计量单位或货币符号。例如：

```cpp
sb->setPrefix("$");
```

要取消前缀显示，只需将该属性设为空字符串。==当设置了`specialValueText()`时，不会为`minimum()`显示前缀。==

| 访问函数      |                                    |
| --------- | ---------------------------------- |
| `QString` | `prefix() const`                   |
| `void`    | `setPrefix(const QString &prefix)` |

### suffix : QString

该属性保存 spin box 的**后缀**。默认值为空字符串。

后缀会被追加到显示值的末尾，通常用于显示计量单位或货币符号。例如：

```cpp
sb->setSuffix(" km");
```

要取消后缀显示，只需将该属性设为空字符串。==当设置了`specialValueText()`时，不会为`minimum()`显示后缀。==

| 访问函数      |                                    |
| --------- | ---------------------------------- |
| `QString` | `suffix() const`                   |
| `void`    | `setSuffix(const QString &suffix)` |


# Public Functions

### 构造和析构

##### `explicit QSpinBox(QWidget *parent = nullptr)`

构造 spin box，最小值为`0`，最大值为`99`，步长为`1`。初值为`0`。父控件为 *parent* 。

##### `virtual ~QSpinBox() noexcept`

析构函数。

# Protected Functions

##### `virtual QString textFromValue(int value) const`

1. 当 spin box 需要==显示给定 *value*== 时调用该虚函数。

2. 默认实现返回的字符串包含使用`QWidget::locale().toString()`以标准方式打印的 *value* ，但会移除千位分隔符，除非调用[`setGroupSeparatorShown()`](https://doc.qt.io/qt-6/qabstractspinbox.html#showGroupSeparator-prop)。

3. 重写该函数可以返回任何内容。如果你重写了该函数，可能还需要重写`valueFromText()`和`validate()`。

> 注意：
> 1. `QSpinBox`不会为[`specialValueText()`](https://doc.qt.io/qt-6/qabstractspinbox.html#specialValueText-prop)调用此函数。
> 2. 函数返回值不包含`prefix()`和`suffix()`。

##### `virtual int valueFromText(const QString &text) const`

1. 当 spin box 需要==将用户输入的 *text* 解释为数值==时调用该虚函数。

2. 如果派生类需要以非数字的形式显示 spin box 的值，则需重写该函数。

> 注意：`QSpinBox`会单独处理[`specialValueText()`](https://doc.qt.io/qt-6/qabstractspinbox.html#specialValueText-prop)；该函数只关心除特殊值文本外的其他数值。

# Signals

##### `void valueChanged(int i)`

当 spin box 的**值**改变时，发出该信号。新数值通过 *i* 传递。

> 注意：这是 [[QSpinBox#Properties#value int]] 的通知信号。

##### `void textChanged(const QString &text)`

当 spin box 的**文本**改变时，发出该信号。新文本通过 *text* 传递，包含`prefix()`和`suffix()`。

