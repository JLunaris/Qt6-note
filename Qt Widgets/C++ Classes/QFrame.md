https://doc.qt.io/qt-6/qframe.html

`QFrame`类是**可以具有边框的控件**的基类。

- Inherits：[[QWidget]]
- Inherited By：[[QAbstractScrollArea]]，[[QLabel]] 等

# Detailed Description

`QMenu`使用框架将菜单”凸显“在屏幕之上；`QProgressBar`具有”凹陷“的外观；`QLable`具有”平坦“的外观。像这样的控件，它们的**框架**（frame）都可以被修改。

```cpp
QLabel label(...);
label.setFrameStyle(QFrame::Panel | QFrame::Raised);
label.setLineWidth(2);

QProgressBar pbar(...);
label.setFrameStyle(QFrame::NoFrame);
```

`QFrame`类也可以被直接使用，来创建没有任何内容的简单占位边框。

**框架风格**由[框架形状](https://doc.qt.io/qt-6/qframe.html#Shape-enum)和[阴影风格](https://doc.qt.io/qt-6/qframe.html#Shadow-enum)指定，用于在视觉上将`QFrame`与周围控件区分开来。这些属性通过`setFrameStyle()`设置，通过`frameStyle()`获取。

- 框架形状包括：`NoFrame`，`Box`，`Panel`，`StyledPanel`，`HLine`，`VLine`
- 阴影风格包括：`Plain`，`Raised`，`Sunken`

框架控件有3个描述边框厚度的属性：`lineWidth`，`midLineWidth`，`frameWidth`。

- `lineWidth`：框架边框的宽度。
- `midLineWidth`：框架中间有一条额外的线，称为中线（mid-line），该线使用第三种颜色来实现特殊的 3D 效果。注意，只有框架形状为`Box`/`HLine`/`VLine`且阴影风格为`Raised`/`Sunken`时才会绘制中线。
- `frameWidth`：由框架风格决定。

框架和框架内容之间的余地可使用`QWidget::setContentsMargins()`函数定制。

下表展示了一些**风格**和**线宽**的组合：

![[Pasted image 20250506173600.png]]

# Properties

## 边框厚度
### lineWidth : int

该属性保存线宽。==默认值为`1`==。

注意如果框架用作分隔线（`HLine`和`VLine`），则它的**总**线宽由`frameWidth`指定。

| 访问函数   |                     |
| ------ | ------------------- |
| `int`  | `lineWidth() const` |
| `void` | `setLineWidth(int)` |
### midLineWidth : int

该属性保存中线（mid-line）的宽度。默认值为`0`。

| 访问函数   |                        |
| ------ | ---------------------- |
| `int`  | `midLineWidth() const` |
| `void` | `setMidLineWidth(int)` |
### frameWidth : const int

该属性保存画出的框架的宽度。

注意框架宽度取决于**框架风格**，而不仅仅是线宽`lineWidth`和中线宽`midLineWidth`。例如，`NoFrame`风格的框架的宽度总是为`0`，而`Panel`风格的框架的宽度等于线宽`lineWidth`。

| 访问函数  |                      |
| ----- | -------------------- |
| `int` | `frameWidth() const` |
## 框架风格

### frameShape : Shape

该属性保存[框架形状](https://doc.qt.io/qt-6/qframe.html#Shape-enum)值。

| 访问函数            |                                |
| --------------- | ------------------------------ |
| `QFrame::Shape` | `frameShape() const`           |
| `void`          | `setFrameShape(QFrame::Shape)` |
### frameShadow : Shadow

该属性保存[框架阴影](https://doc.qt.io/qt-6/qframe.html#Shadow-enum)值。

| 访问函数             |                                  |
| ---------------- | -------------------------------- |
| `QFrame::Shadow` | `frameShadow() const`            |
| `void`           | `setFrameShadow(QFrame::Shadow)` |
## 框架矩形
### frameRect : QRect

该属性保存框架的矩形。

框架的矩形指的是在其中绘制框架的矩形，默认情况下是整个控件。设置矩形**不会**导致控件更新。当控件改变尺寸时，框架矩形会自动调整。

如果将矩形设为空矩形（例如`QRect(0, 0, 0, 0)`），则结果框架矩形与[控件矩形](https://doc.qt.io/qt-6/qwidget.html#rect-prop)相同。

> `QFrame::frameRect()`和`QWidget::geometry()`的区别：==`frameRect()`是相对于`QFrame`控件本身的==；而`geometry()`是相对`QFrame`控件的父控件的。

| 访问函数    |                               |
| ------- | ----------------------------- |
| `QRect` | `frameRect() const`           |
| `void`  | `setFrameRect(const QRect &)` |

# Public Functions

##### `int frameStyle() const`

返回框架风格。默认值为`QFrame::Plain`。

##### `void setFrameStyle(int style)`

将框架风格设为`style`。`style`是[框架形状](https://doc.qt.io/qt-6/qframe.html#Shape-enum)和[框架阴影风格](https://doc.qt.io/qt-6/qframe.html#Shadow-enum)的按位或运算。

如果中线宽度大于`0`，且框架风格为`Raised`/`Sunken`的`Box`/`HLine`/`VLine`，则会绘制中线。绘制中线时使用当前颜色组的 mid-color（即`QPalette::mid()`）。