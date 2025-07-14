https://doc.qt.io/qt-6/qtransform.html

`QTransform`类指定坐标系的 2D 变换。

# Detailed Description

变换（transformation）用于指定如何**平移（translate）**、**缩放（scale）**、**切变（shear）**、**旋转（rotate）**、**投影（project）** 坐标系，通常在图形渲染时使用。

一个`QTransform`对象可以通过`setMatrix()`、`scale()`、`rotate()`、`translate()`和`shear()` 函数构建，也可通过 [basic matrix operations](https://doc.qt.io/qt-6/qtransform.html#basic-matrix-operations) 来构建它。矩阵还可在构造时定义，并可通过`reset()`函数重置为单位矩阵（默认状态）。

`QTransform`类支持图形元素的映射：使用[`map()`](https://doc.qt.io/qt-6/qtransform.html#map)可以将给定的点、线段、多边形、区域或 painter path 映射到**由该矩阵所定义的**坐标系中。对于矩形，其坐标可以通过[`mapRect()`](https://doc.qt.io/qt-6/qtransform.html#mapRect)进行变换；也可以通过[`mapToPolygon()`](https://doc.qt.io/qt-6/qtransform.html#mapToPolygon)将矩形变换为多边形（polygon）（映射到**由该矩阵所定义的**坐标系中）。

`QTransform`提供[`isIdentity()`](https://doc.qt.io/qt-6/qtransform.html#isIdentity)函数，用于判断矩阵是否为单位矩阵；提供[`isInvertible()`](https://doc.qt.io/qt-6/qtransform.html#isInvertible)函数，用于判断矩阵是否可逆（即满足 AB = BA = I）。若矩阵可逆，[`inverted()`](https://doc.qt.io/qt-6/qtransform.html#inverted)函数返回其逆矩阵的副本；否则返回单位矩阵。[`adjoint()`](https://doc.qt.io/qt-6/qtransform.html#adjoint)函数返回该矩阵的伴随矩阵。此外，还提供[`determinant()`](https://doc.qt.io/qt-6/qtransform.html#determinant)函数，用于返回矩阵的行列式值。

最后，`QTransform`类支持矩阵的乘法、加法和减法运算，该类的对象还可被流式操作，也可进行比较操作。

### Basic Matrix Operations

![[Pasted image 20250624171335.png]]

`QTransform`对象包含一个 3 x 3 的矩阵。其中：

- 【平移因子】`m31`（`dx`）和`m32`（`dy`）元素指定**水平平移**和**垂直平移**
- 【缩放因子】`m11`和`m22`元素指定**水平缩放**和**垂直缩放**
- 【切变因子】`m21`和`m12`元素指定**水平切变**和**垂直切变**
- 【投影因子】`m13`和`m23`元素指定**水平投影**和**垂直投影**，`m33`是一个额外的投影因子

`QTransform`使用以下公式将平面中的一个点变换为另一个点：

```
x' = m11*x + m21*y + dx
y' = m22*y + m12*x + dy
if (!isAffine()) {
    w' = m13*x + m23*y + m33
    x' /= w'
    y' /= w'
}
```

其中，`(x, y)`是原始点，`(x', y')`是变换后的点。`(x', y')`也可以变换回`(x, y)`，对`inverted()`函数返回的矩阵执行同样的操作即可。

这些矩阵元素可以在构造矩阵时设置，也可通过`setMatrix()`函数在之后进行设置。它们可以通过`translate()`、`rotate()`、`scale()`和`shear()`等便捷函数进行操作。当前设置的值可以通过以下函数检索：`m11()`、`m12()`、`m13()`、`m21()`、`m22()`、`m23()`、`m31()`、`m32()`、`m33()`，以及`dx()`和`dy()`。

- ==平移（translate）==是最简单的变换：设置`dx`和`dy`会使坐标系在 X 轴方向上移动`dx`个单位，在 Y 轴方向上移动`dy`个单位
- ==缩放（scale）==可通过设置`m11`和`m22`实现。例如，将`m11`设为 2，`m22`设为 1.5，表示宽度缩放为原来的 2 倍，高度缩放为原来的 1.5 倍
- **单位矩阵**中，`m11`、`m22`、`m33`为 1，其余元素为 0，它会将点映射为自身
- ==切变（shear）==由`m12`和`m21`控制，将它们设置为非零值会扭曲坐标系
- ==旋转（rotate）==通过设置**切变因子**和**缩放因子**实现
- ==透视变换（perspective transformation）==则通过设置**投影因子**和**缩放因子**实现

# Public Functions

### 构造函数

##### `QTransform()`

构造一个单位矩阵。

所有元素都被设为`0`，除了`m11`和`m22`（用于指定缩放）和`m33`被设为`1`。

##### `QTransform(qreal m11, qreal m12, qreal m21, qreal m22, qreal dx, qreal dy)`

构造一个矩阵，元素分别为`m11`、`m12`、`m21`、`m22`、`dx`、`dy`。

##### `QTransform(qreal m11, qreal m12, qreal m13, qreal m21, qreal m22, qreal m23, qreal m31, qreal m32, qreal m33)`

构造一个矩阵，元素分别为`m11`、`m12`、`m13`、`m21`、`m22`、`m23`、`m31`、`m32`、`m33`。

### 设置矩阵

##### `void setMatrix(qreal m11, qreal m12, qreal m13, qreal m21, qreal m22, qreal m23, qreal m31, qreal m32, qreal m33)`

将矩阵的各元素设为指定的值：`m11`、`m12`、`m13`，`m21`、`m22`、`m23`，`m31`、`m32`和`m33`。注意，该函数会替换掉之前的所有矩阵值。

`QTransform`提供了[`translate()`](https://doc.qt.io/qt-6/qtransform.html#translate)、[`rotate()`](https://doc.qt.io/qt-6/qtransform.html#rotate)、[`scale()`](https://doc.qt.io/qt-6/qtransform.html#scale)、[`shear()`](https://doc.qt.io/qt-6/qtransform.html#shear)等便捷函数，可基于当前定义的坐标系来操作矩阵的各种元素。

### 重置矩阵

##### `void reset()`

将矩阵重置为**单位矩阵**，即`m11`和`m22`（表示缩放）和`m33`设为`1`，其他所有元素设为`0`。

### 映射

##### `void map(qreal x, qreal y, qreal *tx, qreal *ty) const`

将给定的坐标 *x* 和 *y* 映射到该矩阵定义的坐标系中。结果值分别存储到 *tx* 和 *ty* 中。

坐标按以下公式进行变换：

```
x' = m11*x + m21*y + dx
y' = m22*y + m12*x + dy
if (!isAffine()) {
    w' = m13*x + m23*y + m33
    x' /= w'
    y' /= w'
}
```

`(x, y)`是原始点，`(x', y')`是变换后的点。

##### `QPainterPath map(const QPainterPath &path) const`

重载函数。

创建并返回`QPainterPath`对象，这是将传入的 *path* 映射到该矩阵定义的坐标系中的结果。

##### 其他map函数

```cpp
QLine map(const QLine &l) const
QLineF map(const QLineF &line) const
QPoint map(const QPoint &point) const
QPointF map(const QPointF &p) const
QPolygon map(const QPolygon &polygon) const
QPolygonF map(const QPolygonF &polygon) const
QRegion map(const QRegion &region) const
void map(int x, int y, int *tx, int *ty) const
```

##### `QRectF mapRect(const QRectF &rectangle) const`

创建并返回`QRectF`对象，这是将传入的 *rectangle* 映射到该矩阵定义的坐标系中的结果。

矩形的坐标按以下公式进行变换：

```
x' = m11*x + m21*y + dx
y' = m22*y + m12*x + dy
if (!isAffine()) {
    w' = m13*x + m23*y + m33
    x' /= w'
    y' /= w'
}
```

如果指定了旋转或切变，该函数返回的是==变换后区域的边界矩形==。要获取变换后的精确区域形状，请使用`mapToPolygon()`函数。

##### `QRect mapRect(const QRect &rectangle) const`

重载函数。

变换后的坐标会被四舍五入为最接近的整数值。

##### `QPolygon mapToPolygon(const QRect &rectangle) const`

创建并返回`QPolygon`对象，这是将传入的 *rectangle* 映射到该矩阵定义的坐标系中的结果。

多边形和矩形在变换时行为略有不同（由于整数舍入），因此`matrix.map(QPolygon(rectangle))`和`matrix.mapToPolygon(rectangle)`的结果不一定完全相同。

### 平移

##### `QTransform &translate(qreal dx, qreal dy)`

将坐标系沿着 x 轴移动 *dx* 个单位，沿着 y 轴移动 *dy* 个单位，并返回该变换矩阵的引用。

> 个人笔记：也就是说，它不是移动物体，而是**移动坐标系**（即你后续绘制内容的位置会被偏移）

# Static Public Members

##### `static bool quadToQuad(const QPolygonF &one, const QPolygonF &two, QTransform &trans)`

创建一个==能够将一个四边形 *one* 映射为另一个四边形 *two* ==的变换矩阵 *trans*。如果该变换是可能的，返回`true`；否则返回`false`。

这是一个便捷函数，结合了`quadToSquare()`和`squareToQuad()`。

##### `static bool quadToSquare(const QPolygonF &quad, QTransform &trans)`

创建一个==能够将一个四边形 *quad* 映射为一个单位正方形==的变换矩阵 *trans*。如果该变换是可能的，返回`true`；否则返回`false`。

##### `static bool squareToQuad(const QPolygonF &quad, QTransform &trans)`

创建一个==能够将一个单位正方形映射为一个四边形 *quad* ==的变换矩阵 *trans*。如果该变换是可能的，返回`true`；否则返回`false`。
