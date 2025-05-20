https://doc.qt.io/qt-6/qconicalgradient.html

`QConicalGradient`类与`QBrush`结合使用，以指定**锥形渐变**填充。

- Inherits：[[QGradient]]

# Public Functions

### 构造

##### `QConicalGradient()`

构造一个锥形渐变，中心为`(0, 0)`，并从角度 0 开始进行颜色插值。

##### `QConicalGradient(const QPointF &center, qreal angle)`

构造一个锥形渐变，中心为 *center*，并从角度 *angle* 开始进行颜色插值。

==*angle* 的范围是`[0, 360]`。==

##### `QConicalGradient(qreal cx, qreal cy, qreal angle)`

等价于`QConicalGradient(QPointF(cx, cy), angle)`。

### 中心

##### `void setCenter(const QPointF &center)`

将锥形渐变的**中心**（以逻辑坐标表示）设为 *center*。

##### `QPointF center() const`

返回锥形渐变的**中心**（以逻辑坐标表示）。

### 起始角

##### `void setAngle(qreal angle)`

将锥形渐变的**起始角**（以逻辑坐标表示）设为 *angle*。

##### `qreal angle() const`

返回锥形渐变的**起始角**（以逻辑坐标表示）。