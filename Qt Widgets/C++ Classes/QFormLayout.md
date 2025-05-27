https://doc.qt.io/qt-6/qformlayout.html

`QFormLayout`类用于管理表单（包括输入控件及其关联标签）的布局。

- Inherits：[[QLayout]]

# Detailed Description

`QFormLayout`是一个便捷布局类，将其子控件布局为==二列表单==：左列为**标签(label)**，右列为**字段(field) 控件**（如 line editors、spin boxes 等）。

传统上，这样的二列表单布局是使用`QGridLayout`实现的，而`QFormLayout`是一种更高级的替代方案，具有以下优势：

①遵循不同平台的外观和风格指南：例如，[macOS Aqua](http://developer.apple.com/library/mac/#documentation/UserExperience/Conceptual/AppleHIGuidelines/Intro/Intro.html) 和 KDE 指南指出标签应当右对齐，而 Windows 和 GNOME 应用通常使用左对齐。

②支持自动换行：对于小屏设备，可将`QFormLayout`设置 [wrap long rows](https://doc.qt.io/qt-6/qformlayout.html#RowWrapPolicy-enum)，甚至是 [wrap all rows](https://doc.qt.io/qt-6/qformlayout.html#RowWrapPolicy-enum)。

③具有便捷的API来创建**标签-字段对（label-field pairs）**：`void addRow(QWidget *label, QWidget *field)`函数在幕后创建一个`QLabel`，然后自动设置其 buddy。我们可以这样编写代码：

```cpp
QFormLayout *formLayout = new QFormLayout(this);
formLayout->addRow(tr("&Name:"), nameLineEdit);
formLayout->addRow(tr("&Email:"), emailLineEdit);
formLayout->addRow(tr("&Age:"), ageSpinBox);
```

将其与以下使用`QGridLayout`编写的代码比较：

```cpp
QGridLayout *gridLayout = new QGridLayout(this);

nameLabel = new QLabel(tr("&Name:"));
nameLabel->setBuddy(nameLineEdit);

emailLabel = new QLabel(tr("&Name:"));
emailLabel->setBuddy(emailLineEdit);

ageLabel = new QLabel(tr("&Name:"));
ageLabel->setBuddy(ageSpinBox);

gridLayout->addWidget(nameLabel, 0, 0);
gridLayout->addWidget(nameLineEdit, 0, 1);
gridLayout->addWidget(emailLabel, 1, 0);
gridLayout->addWidget(emailLineEdit, 1, 1);
gridLayout->addWidget(ageLabel, 2, 0);
gridLayout->addWidget(ageSpinBox, 2, 1);
```

下表显示了不同风格的默认外观：

| 风格                                                                                | 示例                                                                         | 说明                                                                                                                                                                       |
| :-------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [QCommonStyle](https://doc.qt.io/qt-6/qcommonstyle.html)派生风格 (除了 QPlastiqueStyle) | ![](https://doc.qt.io/qt-6/images/qformlayout-win.png)<br>                 | 传统风格，用于Windows、GNOME和早期版本的KDE。标签左对齐，可扩展输入框会自动变长以填满横向空间。（这通常就是使用二列`QGridLayout`得到的效果）                                                                                     |
| QMacStyle                                                                         | ![](https://doc.qt.io/qt-6/images/qformlayout-mac.png)<br>                 | 基于[macOS Aqua](http://developer.apple.com/library/mac/#documentation/UserExperience/Conceptual/AppleHIGuidelines/Intro/Intro.html)指南的风格。标签右对齐，输入框长度不会超出其指定长度，并且整个表单水平居中。 |
| QPlastiqueStyle                                                                   | ![](https://doc.qt.io/qt-6/images/qformlayout-kde.png)<br>                 | KDE应用的推荐风格。类似于MacStyle，除了表单是左对齐的，且输入框会自动填满横向空间。                                                                                                                          |
| Qt Extended styles                                                                | ![](https://doc.qt.io/qt-6/images/qformlayout-qpe.png)<br>================ | Qt Extended风格的默认风格。标签右对齐，可扩展输入框会自动变长以填满横向空间，对于长的行启用了自动换行。                                                                                                                |

表单风格也可通过调用[`setLabelAlignment()`](https://doc.qt.io/qt-6/qformlayout.html#labelAlignment-prop)、[`setFormAlignment()`](https://doc.qt.io/qt-6/qformlayout.html#formAlignment-prop)、[`setFieldGrowthPolicy()`](https://doc.qt.io/qt-6/qformlayout.html#fieldGrowthPolicy-prop)、[`setRowWrapPolicy()`](https://doc.qt.io/qt-6/qformlayout.html#rowWrapPolicy-prop)来单独覆盖。例如，若要在所有平台上模拟`QMacStyle`的表单布局，但标签左对齐，可以这样写：

```cpp
formLayout->setRowWrapPolicy(QFormLayout::DontWrapRows);
formLayout->setFieldGrowthPolicy(QFormLayout::FieldsStayAtSizeHint);
formLayout->setFormAlignment(Qt::AlignHCenter | Qt::AlignTop);
formLayout->setLabelAlignment(Qt::AlignLeft);
```

# Properties

### horizontalSpacing : int

该属性保存**水平**排列控件之间的间距。

默认情况下，若未显式设置该值，则布局的水平间距继承自父级布局，或父控件的样式（style）设置。

| 访问函数   |                                     |
| ------ | ----------------------------------- |
| `int`  | `horizontalSpacing() const`         |
| `void` | `setHorizontalSpacing(int spacing)` |

### verticalSpacing : int

该属性保存**垂直**排列控件之间的间距。

默认情况下，若未显式设置该值，则布局的垂直间距继承自父级布局，或父控件的样式（style）设置。

| 访问函数   |                                   |
| ------ | --------------------------------- |
| `int`  | `verticalSpacing() const`         |
| `void` | `setVerticalSpacing(int spacing)` |

# Public Functions

### 构造和析构

##### `explicit QFormLayout(QWidget *parent = nullptr)`

构造一个表单布局，父对象为 *parent*。

该布局直接被设为 *parent* 的**顶级布局**。控件只能有一个顶级布局，由`QWidget::layout()`返回。

##### `virtual ~QFormLayout() noexcept`

销毁该表单布局。

### 增加行

##### `void addRow(QWidget *label, QWidget *field)`

在该表单布局底部添加一个新行，使用给定的==标签 *label* 和字段 *field*==。

##### `void addRow(QLayout *layout)`

将指定的 *layout* 添加到此表单布局的末尾。==*layout* 会横跨整个二列==（标签列 + 字段控件列）。

##### `void addRow(QWidget *widget)`

将指定的 *widget* 添加到此表单布局的末尾。==*widget* 会横跨整个二列==（标签列 + 字段控件列）。

##### `void addRow(QWidget *label, QLayout *field)`

重载函数。

##### `void addRow(const QString &labelText, QLayout *field)`

该重载函数会自动在幕后创建一个以 *labelText* 为文本的`QLabel`。

【案例——字段控件后带单位的表单】

```cpp
QHBoxLayout *hLayout {new QHBoxLayout};  
hLayout->addWidget(m_hField);  
hLayout->addWidget(new QLabel {"度"});  
hLayout->setSpacing(3);  
formLayout->addRow("色相(H):", hLayout);
```

显示效果：![[Pasted image 20250527101443.png]]

##### `void addRow(const QString &labelText, QWidget *field)`

该重载函数会自动在幕后创建一个以 *labelText* 为文本的`QLabel`，并将 *field* 设为该`QLabel`的 [buddy](https://doc.qt.io/qt-6/qlabel.html#setBuddy)。