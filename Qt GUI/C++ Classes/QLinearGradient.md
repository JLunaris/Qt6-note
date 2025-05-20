https://doc.qt.io/qt-6/qlineargradient.html

`QLinearGradient`类与`QBrush`结合使用，以指定**线性渐变**填充。

- Inherits：[[QGradient]]

# Detailed Description

线性渐变在**起始点**和**终止点**之间进行颜色插值。在这些点之外，渐变可以**填充**（pad）、**反射**（reflect）或**重复**（repeat），取决于当前设置的蔓延（[spread](https://doc.qt.io/qt-6/qgradient.html#Spread-enum)）方法：

| [PadSpread](https://doc.qt.io/qt-6/qgradient.html#Spread-enum) (默认) | [ReflectSpread](https://doc.qt.io/qt-6/qgradient.html#Spread-enum) | [RepeatSpread](https://doc.qt.io/qt-6/qgradient.html#Spread-enum) |
| ------------------------------------------------------------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------- |
| ![](https://doc.qt.io/qt-6/images/qlineargradient-pad.png)          | ![](https://doc.qt.io/qt-6/images/qlineargradient-reflect.png)     | ![](https://doc.qt.io/qt-6/images/qlineargradient-repeat.png)     |

==渐变中的颜色通过**停止点**（stop point）定义==，内部使用[`QGradientStop`](https://doc.qt.io/qt-6/qgradient.html#QGradientStop-typedef)型对象存储此信息（即位置和颜色构成的`std::pair<qreal, QColor>`）。使用[`QGradient::setColorAt()`](https://doc.qt.io/qt-6/qgradient.html#setColorAt)或[`QGradient::setStops()`](https://doc.qt.io/qt-6/qgradient.html#setStops)来设置停止点。==整个停止点集合描述了渐变区域该如何填充==。如果未指定任何停止点，则默认使用从 **0处的黑色** 到 **1处的白色** 的渐变。

除了从`QGradient`类继承的函数外，`QLinearGradient`类还提供`finalStop()`函数返回渐变的终止点，以及`start()`函数返回渐变的起始点。

# Public Functions

### 构造
##### `QLinearGradient()`

构造一个线性渐变，插值区域为`(0, 0)`和`(1, 1)`两点确定的区域。

##### `QLinearGradient(const QPointF &start, const QPointF &finalStop)`

构造一个线性渐变，插值区域为 *start* 和 *finalStop*。

> *start* 和 *finalStop* 两点确定一个矩形区域。

##### `QLinearGradient(qreal x1, qreal y1, qreal x2, qreal y2)`

等价于 `QLinearGradient(QPointF(x1, y1), QPointF(x2, y2))`。

### 起始点

##### `void setStart(const QPointF &start)`

将线性渐变的**起始点**（以逻辑坐标表示）设为 *start*。

##### `QPointF start() const`

返回线性渐变的**起始点**（以逻辑坐标表示）。

### 终止点

##### `void setFinalStop(const QPointF &stop)`

将线性渐变的**终止点**（以逻辑坐标表示）设为 *stop*。

##### `QPointF finalStop() const`

返回线性渐变的**终止点**（以逻辑坐标表示）。