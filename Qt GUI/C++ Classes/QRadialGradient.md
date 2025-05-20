https://doc.qt.io/qt-6/qradialgradient.html

`QRadialGradient`类与`QBrush`结合使用，以指定**径向渐变**填充。

- Inherits：[[QGradient]]

# Public Functions

### 构造

##### `QRadialGradient()`

构造一个==简单径向渐变==，中心和焦点都为`(0, 0)`，它们的半径为`1`。

##### `QRadialGradient(const QPointF &center, qreal radius)`

构造一个==简单径向渐变==，中心为 *center*，半径为 *radius*，焦点和圆心重合。

##### `QRadialGradient(const QPointF &center, qreal radius, const QPointF &focalPoint)`

构造一个==简单径向渐变==，中心为 *center*，半径为 *radius*，焦点为 *focalPoint*。

> 注意：如果给定的焦点在由 *center* 和 *radius* 确定的圆之外，它会被重新调整到 *center* 到 *focalPoint* 的连线与圆的交点上。

##### `QRadialGradient(const QPointF &center, qreal centerRadius, const QPointF &focalPoint, qreal focalRadius)`

构造一个==扩展径向渐变==。参数分别是：中心、中心半径、焦点、焦点半径。
### 中心

##### `void setCenter(const QPointF &center)`

将径向渐变的**中心**（以逻辑坐标表示）设为 *center*。

##### `QPointF center() const`

返回径向渐变的**中心**（以逻辑坐标表示）。

### 中心半径

##### `void setRadius(qreal radius)`

将径向渐变的**中心半径**（以逻辑坐标表示）设为 *radius*。

等价于[`setCenterRadius()`](https://doc.qt.io/qt-6/qradialgradient.html#setCenterRadius)。

##### `qreal radius() const`

返回径向渐变的**中心半径**（以逻辑坐标表示）。

等价于[`centerRadius()`](https://doc.qt.io/qt-6/qradialgradient.html#centerRadius)。

### 焦点

##### `void setFocalPoint(const QPointF &focalPoint)`

将径向渐变的**焦点**（以逻辑坐标表示）设为 *focalPoint*。

##### `QPointF focalPoint() const`

返回径向渐变的**焦点**（以逻辑坐标表示）。

### 焦点半径

##### `void setFocalRadius(qreal radius)`

将径向渐变的**焦点半径**（以逻辑坐标表示）设为 *radius*。

##### `qreal focalRadius() const`

返回径向渐变的**焦点半径**（以逻辑坐标表示）。