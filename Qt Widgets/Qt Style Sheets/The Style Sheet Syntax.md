https://doc.qt.io/qt-6/stylesheet-syntax.html

Qt样式表术语和语法规则与HTML CSS几乎相同。

# Style Rules

**样式表**（Style Sheet）由一系列**样式规则**（style rule）组成。样式规则由**选择器**（selector）和**声明**（declaration）组成。选择器指定哪些控件受规则影响；声明指定应在控件上设置哪些属性。例如：

```CSS
QPushButton { color: red }
```

上面的样式规则中，`QPushButton`是选择器，`{ color: red }`是声明。规则指定`QPushButton`及其子类（例如`MyPushBotton`）应当使用红色作为前景色。

Qt样式表通常**不区分大小写**（也就是说`color`,`Color`,`COLOR`,`cOloR`代表相同的属性）。唯一的例外是类名、[对象名](https://doc.qt.io/qt-6/qobject.html#objectName-prop)和**Qt属性名**（即每个Qt C++类文档的Properties栏下的属性），它们区分大小写。

**可以为同一个声明指定多个选择器**，使用逗号（`,`）分隔选择器。例如：

```CSS
QPushButton, QLineEdit, QComboBox { color: red }
```

等价于下面三条规则的序列：

```CSS
QPushButton { color: red }
QLineEdit { color: red }
QComboBox { color: red }
```

样式规则的声明部分是 **property: value** 对的列表，用大括号（`{}`）包围，用分号（`;`）分隔。例如：

```CSS
QPushButton { color: red; background-color: white }
```

有关Qt控件提供的属性列表，详见[List of Properties](https://doc.qt.io/qt-6/stylesheet-reference.html#list-of-properties)。

# Selector Types

到目前为止的所有示例，都使用了最简单的选择器——类型选择器（Type Selector）。Qt样式表支持[CSS2中的所有选择器](http://www.w3.org/TR/REC-CSS2/selector.html#q1)，下表总结了最常用的选择器类型。


| Selector            | Example                     | Explanation                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| :------------------ | :-------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Universal Selector  | `*`                         | 匹配所有控件                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Type Selector       | `QPushButton`               | 匹配`QPushButton`及其子类的实例                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Property Selector   | `QPushButton[flat="false"]` | 匹配所有[flat属性](https://doc.qt.io/qt-6/qpushbutton.html#flat-prop)为false的`QPushButton`实例。<br><br>可使用该选择器测试任何**支持[`QVariant::toString()`](https://doc.qt.io/qt-6/qvariant.html#toString)的**Qt属性。此外，还支持`class`属性，表示类名。<br><br>该选择器还可用于测试动态属性。有关如何使用动态属性进行定制，详见[Customizing Using Dynamic Properties](https://doc.qt.io/qt-6/stylesheet-examples.html#customizing-using-dynamic-properties)。<br><br>除了使用`=`，还可使用`~=`来测试`QStringList`型的Qt属性是否包含给定的`QString`。<br><br>【警告】如果Qt属性的值在样式表设置后发生更改，可能需要强制重新计算样式表。一种方式是先 unset 样式表，然后再 set 它。 |
| Class Selector      | `.QPushButton`              | 匹配`QPushButton`的实例，但不匹配其子类的实例。<br><br>这等价于`*[class~="QPushButton"]`。                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ID Selector         | `QPushButton#okButton`      | 匹配`QPushButton`实例中所有对象名为`okButton`的实例                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Descendant Selector | `QDialog QPushButton`       | 匹配`QPushButton`实例中所有是`QDialog`的后代（如子对象、孙子对象）的实例。                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Child Selector      | `QDialog > QPushButton`     | 匹配`QPushButton`实例中所有是`QDialog`的直接子对象的实例。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
# Sub-Controls

对于复杂控件的样式定制，需要访问其 subcontrol，例如`QComboBox`的下拉按钮或`QSpinBox`的上下箭头。选择器可以包含 **subcontrol**，限制规则仅应用到控件的 subcontrol 上。例如：

```CSS
QComboBox::drop-down { image: url(dropdown.png) }
```

上述规则为所有`QComboBox`的下拉按钮设置风格。虽然双冒号（`::`）句法让人想起CSS3伪元素，但 Qt **子控制**（Sub-Controls）在概念上与其不同，且拥有不同的层叠语义。

---

子控制始终相对另一个元素（参考元素）定位，参考元素可以是**控件**或**另一个子控制**。例如，`QComboBox`的`::drop-down`默认位于`QComboBox`的 padding rectangle 的右上角。`::drop-down`默认位于`::drop-down`子控制的 contents rectangle 的中心。可样式化控件及其子控制的默认位置，详见[List of Stylable Widgets](https://doc.qt.io/qt-6/stylesheet-reference.html#list-of-stylable-widgets)。

使用[`subcontrol-origin`](https://doc.qt.io/qt-6/stylesheet-reference.html#subcontrol-origin)属性可以改变最初使用的矩形。例如，如果我们想要将下拉按钮置于`QComboBox`的 margin rectangle 而不是默认的 padding rectangle 中，我们可以指定：

```CSS
QComboBox {
	margin-right: 20px;
}
QComboBox::drop-down {
	subcontrol-origin: margin;
}
```

使用 [`subcontrol-position`](https://doc.qt.io/qt-6/stylesheet-reference.html#subcontrol-position)属性可以改变下拉按钮在 margin rectangle 中的对齐方式。

[width](https://doc.qt.io/qt-6/stylesheet-reference.html#width)和[height](https://doc.qt.io/qt-6/stylesheet-reference.html#height)属性可用于控制子控制的尺寸。请注意，设置图像将隐式设置子控制的尺寸。

---

[**相对定位方案**](https://doc.qt.io/qt-6/stylesheet-reference.html#position)（`position: relative`）允许子控制根据其初始位置进行偏移。例如，当`QComboBox`的下拉按钮被按下时，我们可能希望其中的箭头图标略微偏移，以产生“按下”的效果。为实现这一点，我们可以指定：

```CSS
QComboBox::down-arrow {
	image: url(down_arrow.png);
}
QComboBox::down-arrow:pressed {
	position: relative;
	top: 1px;
	left: 1px;
}
```

[**绝对定位方案**](https://doc.qt.io/qt-6/stylesheet-reference.html#position)（`position: absolute`）允许子控制根据参考元素改变其位置和尺寸。

一旦完成定位，子控制会被当成控件来处理，并且可使用**盒子模型**来设置样式。

---

Qt 支持的所有子控制，详见[List of Sub-Controls](https://doc.qt.io/qt-6/stylesheet-reference.html#list-of-sub-controls)。更实际的案例见[Customizing the QPushButton's Menu Indicator Sub-Control](https://doc.qt.io/qt-6/stylesheet-examples.html#customizing-the-qpushbutton-s-menu-indicator-sub-control)。

> 注意：对于像`QComboBox`、`QScrollBar`这样的复杂控件，如果你定制了它的某个属性或子控制，那么你必须同时定制它的其余**所有**属性或子控制。


# Pseudo-States

选择器可以包含伪状态（pseudo-states），表示根据控件的状态来限制规则的应用范围。伪状态出现在**选择器的末尾**，中间有一个冒号（`:`）。例如，下面的规则仅在鼠标悬停在`QPushButton`上时生效：

```CSS
QPushButton:hover { color: white }
```

伪状态可以使用感叹号（`!`）进行**取反**。例如，下面的规则在鼠标**未悬停**在`QRadioButton`上时生效：

```CSS
QRadioButton:!hover { color: red }
```

伪状态可以被链起来，此时隐含**逻辑AND**。例如，下面的规则在鼠标**悬停**在**被选中的**`QCheckBox`上时生效：

```CSS
QCheckBox:hover:checked { color: white }
```

取反的伪状态可以出现在伪状态链中。例如，下面的规则在鼠标悬停在`QPushButton`上，且按钮未被按下时生效：

```CSS
QPushButton:hover:!pressed { color:blue; }
```

使用逗号（`,`）来表示**逻辑OR**：

```CSS
QCheckBox:hover, QCheckBox:checked { color: white }
```

伪状态可以与 subcontrol 结合使用。例如：

```CSS
QComboBox::drop-down:hover { image: url(dropdown_bright.png) }
```

Qt 控件提供的所有伪状态，详见[List of Pseudo-States](https://doc.qt.io/qt-6/stylesheet-reference.html#list-of-pseudo-states)。