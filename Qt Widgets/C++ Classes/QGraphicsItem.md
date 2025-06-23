https://doc.qt.io/qt-6/qgraphicsitem.html

`QGraphicsScene`中所有**图元**的基类都是`QGraphicsItem`。

- Inherited By：[QGraphicsLineItem](https://doc.qt.io/qt-6/qgraphicslineitem.html)、[[QGraphicsObject]]

# Public Functions

### 构造和析构

##### `explicit QGraphicsItem(QGraphicsItem *parent = nullptr)`

构造一个`QGraphicsItem`，父图元为 *parent*。

如果 *parent* 为`nullptr`，可以通过调用`QGraphicsScene::addItem()`将该图元添加到场景中，此时该图元将成为一个顶级图元。

##### `virtual ~QGraphicsItem() noexcept`

销毁该图元==及其所有子图元==。如果该图元当前与某个场景关联，它会先被场景移除，再被删除。

> 注意：在销毁图元前，先将其从`QGraphicsScene`中移除会更高效。

### 缓存模式

##### `void setCacheMode(QGraphicsItem::CacheMode mode, const QSize &logicalCacheSize = QSize())`

将图元的缓存模式设为 *mode*。

相关内容：[QGraphicsItem::CacheMode](https://doc.qt.io/qt-6/qgraphicsitem.html#CacheMode-enum)

可选的 *logicalCacheSize* 实参仅用于[`ItemCoordinateCache`](https://doc.qt.io/qt-6/qgraphicsitem.html#CacheMode-enum)模式（其他模式将忽略该参数），描述缓存缓冲区的分辨率；如果 *logicalCacheSize* 为`(100,100)`，则`QGraphicsItem`会将图元适配到图形内存中的 100×100 像素，而不管图元本身的逻辑大小。默认情况下`QGraphicsItem`使用`boundingRect()`的尺寸作为 *logicalCacheSize*。

如果图元花费大量时间重绘自身，启用缓存可以加速渲染。但在某些情况下，缓存会降低渲染速度，尤其是当图元重绘所需的时间小于从缓存中重绘时。

当启用缓存时，图元的[`paint()`](https://doc.qt.io/qt-6/qgraphicsitem.html#paint)函数通常会画在一个屏幕外的 pixmap 缓存中；对于后续的任何重绘请求，Graphics View framework 将直接从缓存中重绘。这种方法与`QGLWidget`配合使用时效果很好，它将所有缓存存储为 OpenGL 纹理。

请注意，可能需要更改[`QPixmapCache`](https://doc.qt.io/qt-6/qpixmapcache.html)的缓存限制，以获得最佳性能。

> 注意：启用缓存并不意味着图元的[`paint()`](https://doc.qt.io/qt-6/qgraphicsitem.html#paint)函数仅在显式调用[`update()`](https://doc.qt.io/qt-6/qgraphicsitem.html#update)时才调用。例如，在内存资源紧张的情况下，Qt 可能会决定丢掉部分缓存信息；此时，即使没有调用`update()`，图元的`paint()`函数也可能会被调用（就像未启用缓存一样）。

### 标志

##### `void setFlag(QGraphicsItem::GraphicsItemFlag flag, bool enabled = true)`

如果 *enabled* 为`true`，则启用 *flag*；否则禁用。

相关内容：[QGraphicsItem::GraphicsItemFlag](https://doc.qt.io/qt-6/qgraphicsitem.html#GraphicsItemFlag-enum)

##### `void setFlags(QGraphicsItem::GraphicsItemFlags flags)`

将图元标志设为 *flags*。*flags* 中包含的所有标志都被启用，不在 *flags* 内的标志都被禁用。

如果图元当前拥有焦点，而 *flags* 未启用[`ItemIsFocusable`](https://doc.qt.io/qt-6/qgraphicsitem.html#GraphicsItemFlag-enum)，则调用此函数后图元将失去焦点。同样地，如果图元当前被选中，而 *flags* 未启用[`ItemIsSelectable`](https://doc.qt.io/qt-6/qgraphicsitem.html#GraphicsItemFlag-enum)，则图元将自动取消选中。

默认情况下，不启用任何标志。（[`QGraphicsWidget`](https://doc.qt.io/qt-6/qgraphicswidget.html)默认会启用[`ItemSendsGeometryChanges`](https://doc.qt.io/qt-6/qgraphicsitem.html#GraphicsItemFlag-enum)标志，以便跟踪位置变化。）

相关内容：[QGraphicsItem::GraphicsItemFlag](https://doc.qt.io/qt-6/qgraphicsitem.html#GraphicsItemFlag-enum)

##### `QGraphicsItem::GraphicsItemFlags flags() const`

返回图元的标志。标志描述了该图元启用了哪些可配置的特性。例如，如果标志中包含[`ItemIsFocusable`](https://doc.qt.io/qt-6/qgraphicsitem.html#GraphicsItemFlag-enum)，则该图元可接受输入焦点。

默认情况下，不启用任何标志。

### 选中

##### `void setSelected(bool selected)`

如果 *selected* 为`true`，且图元是**可选中的**，则图元被选中；否则被取消选中。

==如果图元属于某个组，该函数会切换整个组的选中状态==。若组被选中，组内所有图元也都会被选中；若组未被选中，组内没有任何图元会被选中。

==只有 visible、enabled 且 selectable 的图元可以被选中。==如果 *selected* 为`true`，但该图元是invisible 或 disabled 或 unselectable 的，则该函数不做任何事。

默认情况下，图元是不可选中的。要启用选中功能，需设置[`ItemIsSelectable`](https://doc.qt.io/qt-6/qgraphicsitem.html#GraphicsItemFlag-enum)标志。

该函数是便捷函数，允许单独切换**某一个**图元的选中状态。然而，选中图元的更常见的方式是调用[`QGraphicsScene::setSelectionArea()`](https://doc.qt.io/qt-6/qgraphicsscene.html#setSelectionArea)，它会为场景中指定区域内的所有 visible、enabled 且 selectable 的图元调用该函数。

##### `bool isSelected() const`

如果图元被选中，返回`true`；否则返回`false`。

组内的图元会**继承**组的选中状态。

默认情况下，图元未被选中。

### 绘制

##### `virtual void paint(QPainter *painter, const QStyleOptionGraphicsItem *option, QWidget *widget = nullptr) = 0`

这是一个==纯虚函数==。该函数常被 [[QGraphicsView]] 调用，用于在局部坐标系中绘制图元的内容。

参数说明：

- 在派生类中重写该函数，以使用 *painter* 提供图元的绘制实现。
- 参数 *option* 提供图元的样式，例如其状态、暴露区域以及细节层面的提示。
- *widget* 实参是可选的：如果提供，则指向the widget that is being painted on；默认值为`nullptr`。对于缓存绘制，*widget* 始终为`nullptr`。

```cpp
void RoundRectItem::paint(QPainter *painter,
                          const QStyleOptionGraphicsItem *option,
                          QWidget *widget)
{
    painter->drawRoundedRect(-10, -10, 20, 20, 5, 5);
}
```

*painter* 的 pen 的宽度默认为0，且使用绘制设备的调色板中的[`QPalette::Text`](https://doc.qt.io/qt-6/qpalette.html#ColorRole-enum)进行初始化；brush 被初始化为[`QPalette::Window`](https://doc.qt.io/qt-6/qpalette.html#ColorRole-enum)。

==请确保所有的绘制都在[`boundingRect()`](https://doc.qt.io/qt-6/qgraphicsitem.html#boundingRect)边界内==，以避免渲染伪影（因为`QGraphicsView`不会自动为你裁剪 painter）。特别地，==当`QPainter`使用设置好的`QPen`渲染一个形状的轮廓时，轮廓一半会画在外面，一半会画在里面==（例如，使用宽度为2的 pen 时，必须将轮廓绘制在[`boundingRect()`](https://doc.qt.io/qt-6/qgraphicsitem.html#boundingRect)内缩1单位的位置）。==`QGraphicsItem`中，[cosmetic pen](https://doc.qt.io/qt-6/qpen.html#setCosmetic) 的宽度必须为 0。==

所有绘制都发生在**局部坐标系**中。

> 注意：该行为是强制的——图元将总是以**完全相同的方式**重绘自身，除非调用了[`QGraphicsItem::update()`](https://doc.qt.io/qt-6/qgraphicsitem.html#update)；否则可能会出现视觉伪影。换句话说，连续两次对`paint()`的调用，必须产生完全相同的输出，除非两次调用中间调用了`update()`。

> 注意：为图元启用缓存并不能保证`paint()`只会被Graphics View框架调用一次，即使没有显式调用[`update()`](https://doc.qt.io/qt-6/qgraphicsitem.html#update)。详见[`setCacheMode()`](https://doc.qt.io/qt-6/qgraphicsitem.html#setCacheMode)。

### 边界矩形

##### `virtual QRectF boundingRect() const = 0`

这是一个==纯虚函数==，用于定义图元的外边界矩形；==所有绘制都必须限制在该矩形范围内==。[[QGraphicsView]] 使用该函数来决定图元是否需要重绘。

尽管图元的形状可以是任意的，但边界矩形始终是矩形，并且不会受图元的变换的影响。

==如果你想修改图元的边界矩形，必须先调用[`prepareGeometryChange()`](https://doc.qt.io/qt-6/qgraphicsitem.html#prepareGeometryChange)==。这会通知场景该图元即将发生变化，以便场景**更新图元的几何信息索引**；否则场景将无法意识到图元的新几何信息，结果将是未定义的（通常表现为视图中出现渲染伪影）。

重写此函数，以便`QGraphicsView`决定控件的哪一部分（if any）需要重绘。

注意：对于带轮廓的图元形状来说，边界矩形==需要额外补偿画笔宽度的一半（因为要确保整个轮廓都在边界矩形内）==。不过，不需要为抗锯齿效果做额外补偿。例如下面的代码：

```cpp
QRectF CircleItem::boundingRect() const
{
    qreal penWidth = 1;
    return QRectF(-radius - penWidth / 2, -radius - penWidth / 2,
                  diameter + penWidth, diameter + penWidth);
}
```

### 形状

##### `virtual QPainterPath shape() const`

返回图元的形状，是**局部坐标系**中的`QPainterPath`。形状用于许多事情，包括**碰撞检测**（collision detection）、**命中测试**（hit test），以及用于[`QGraphicsScene::items()`](https://doc.qt.io/qt-6/qgraphicsscene.html#items)函数。

==默认实现会调用`boundingRect()`返回一个简单的矩形==，但派生类可以重写该函数，从而为非矩形图元返回一个更精确的形状。例如，圆形图元可以返回一个椭圆形状，以提高碰撞检测的准确性：

```cpp
QPainterPath RoundItem::shape() const
{
    QPainterPath path;
    path.addEllipse(boundingRect());
    return path;
}
```

形状的轮廓可能会因绘制时使用的画笔的宽度和样式而有所不同。如果你希望将该轮廓也包含在图元的形状中，可使用[`QPainterPathStroker`](https://doc.qt.io/qt-6/qpainterpathstroker.html)根据描边创建一个形状。

==该函数会被[`contains()`](https://doc.qt.io/qt-6/qgraphicsitem.html#contains)和[`collidesWithPath()`](https://doc.qt.io/qt-6/qgraphicsitem.html#collidesWithPath)的默认实现所调用==。

# Protected Functions

##### `void prepareGeometryChange()`

为图元的几何信息变化做准备。在修改图元的边界矩形前，调用该函数以确保`QGraphicsScene`的索引保持最新。

如有必要，`prepareGeometryChange()`会自动调用[`update()`](https://doc.qt.io/qt-6/qgraphicsitem.html#update)。

案例：

```cpp
void CircleItem::setRadius(qreal newRadius)
{
    if (radius != newRadius) {
        prepareGeometryChange();
        radius = newRadius;
    }
}
```

