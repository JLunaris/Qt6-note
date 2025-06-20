https://doc.qt.io/qt-6/qpainterpath.html

`QPainterPath`类用于绘制操作，使**图形形状**可以被构建并重复使用。

# Public Functions

### 构造和析构

##### `QPainterPath() noexcept`

构造一个**空的**`QPainterPath`对象。

##### `explicit QPainterPath(const QPointF &startPoint)`

创建`QPainterPath`对象，*startPoint* 是路径的当前位置。

##### `QPainterPath(const QPainterPath &path)`

拷贝构造函数。

##### `~QPainterPath() noexcept`

析构函数。

### 边界矩形 & 控制点矩形

##### `QRectF boundingRect() const`

返回该 painter path 的边界矩形，浮点数精度。

##### `QRectF controlPointRect() const`

返回包含此路径中所有**点**和**控制点**的矩形。

该函数的计算速度**明显快于**精确的`boundingRect()`，并且返回的矩形==始终是`boundingRect()`返回矩形的超集==。

> 【个人笔记】
> 
> `boundingRect()`精确包围路径所有可见内容，但速度慢；
> ![[Pasted image 20250620164914.png]]
> 
>`controlPointRect()`仅根据点和控制点来确定矩形，速度快，但不够精确（但它一定是`boundingRect()`返回矩形的超集）。
>![[Pasted image 20250620164947.png]]

### 添加形状到路径

##### `void addRect(const QRectF &rectangle)`

将 *rectangle* 添加到路径中，作为一个闭合的子路径。

*rectangle* 被作为一组**顺指针方向**的直线添加到路径中，添加完成后，路径的当前点会定位在矩形的**左上角**。

![[Pasted image 20250529162438.png]]

案例：

```cpp
QLinearGradient myGradient;
QPen myPen;
QRectF myRectangle;

QPainterPath myPath;
myPath.addRect(myRectangle);

QPainter painter(this);
painter.setBrush(myGradient);
painter.setPen(myPen);
painter.drawPath(myPath);
```

### 路径运算

##### `QPainterPath subtracted(const QPainterPath &p) const`

==当前路径的填充区域== **减去** ==*p* 的填充区域==，得到一个新路径，返回该新路径。

路径的集合运算会将路径视为**区域**。**非闭合路径会被视为隐式闭合的**。由于贝塞尔曲线相交运算的数值不稳定性，贝塞尔曲线可能会被离散化为线段（line segments）来处理。


