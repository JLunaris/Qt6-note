https://doc.qt.io/qt-6/qvector2d.html

`QVector2D`类表示二维空间中的==矢量（vector）==或==顶点（vertex）==。

# Public Functions

### 构造函数

##### `constexpr QVector2D() noexcept`

构造一个空矢量，使用坐标`(0, 0)`。

##### `explicit constexpr QVector2D(QPoint point) noexcept`

构造一个矢量，使用 *point* 。

##### `explicit constexpr QVector2D(QPointF point) noexcept`

构造一个矢量，使用 *point* 。

##### `constexpr QVector2D(float xpos, float ypos) noexcept`

构造一个矢量，使用坐标`(xpos, ypos)`。两个坐标值必须是有限的。

### 长度

##### `float length() const noexcept`

返回矢量的长度。

### 单位矢量

##### `void normalize() noexcept`

**就地**归一化当前矢量。

如果矢量为空矢量，或矢量长度非常接近`1`，则不做任何事。

##### `QVector2D normalized() const noexcept`

返回该矢量的归一化形式（即单位矢量）。

如果矢量为空矢量，则返回空矢量。如果矢量的长度非常接近`1`，则矢量按原样返回。

### 分量

##### `constexpr void setX(float x) noexcept`

##### `constexpr void setY(float y) noexcept`

##### `float x() const`

##### `float y() const`

### 终点

##### `constexpr QPointF toPointF() const noexcept`

返回该二维矢量的`QPointF`形式。

##### `constexpr QPoint toPoint() const noexcept`

返回该二维矢量的`QPoint`形式。每个坐标被**四舍五入**为最接近的整数。


