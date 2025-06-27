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

### 堆叠顺序

##### `void setZValue(qreal z)`

将图元的 Z 值设置为 *z* 。Z 值决定了**兄弟图元**之间的**堆叠顺序**。Z 值较高的兄弟图元总是绘制在 Z 值较低的兄弟图元之上。

==如果你将 Z 值恢复为之前的值，则图元的插入顺序将决定其堆叠顺序。==

Z 值不会以任何方式影响图元的尺寸。

默认的 Z 值是 `0`。

##### `qreal zValue() const`

返回图元的 Z 值。默认的 Z 值是 `0`。

##### `void stackBefore(const QGraphicsItem *sibling)`

将此图元堆叠在 *sibling* 前，*sibling* 必须是**兄弟图元**（即两图元必须有同一个父图元，或两者都是顶层图元）。==*sibling* 必须与该图元具有相同的 Z 值==，否则调用该函数不会产生任何影响。

默认情况下，所有兄弟图元**按插入顺序堆叠**（即先添加的图元先绘制，后添加的图元后绘制）。如果两个图元的 Z 值不同，则 Z 值较高的图元绘制在上方；如果 Z 值相同，则插入顺序决定堆叠顺序。

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

### 位置

##### `QPointF pos() const`

返回图元在**父坐标系**中的位置。如何图元没有父图元，则返回它在**场景坐标系**中的位置。

图元的位置描述了其==原点（局部坐标`(0, 0)`）在父坐标系中的位置==；该函数返回的值和`mapToParent(0, 0)`相等。

为了方便，你可以调用[`scenePos()`](https://doc.qt.io/qt-6/qgraphicsitem.html#scenePos)来获取图元在**场景坐标系**中的位置，而不考虑其父图元。

##### `qreal x() const`

等价于`pos().x()`。

##### `qreal y() const`

等价于`pos().y()`。

##### `void setPos(const QPointF &pos)`

将图元的位置设为 *pos*，相对于**父坐标系**。如果没有父图元，则 *pos* 是在**场景坐标系**中的。

图元的位置描述了其原点（局部坐标`(0, 0)`）在父坐标系中的位置。

##### `void setX(qreal x)`

等价于`setPos(x, y())`。

##### `void setY(qreal y)`

等价于`setPos(x(), y)`。

##### `QPointF scenePos() const`

返回图元在**场景坐标系**中的位置。等价于调用`mapToScene(0, 0)`。

### 子图元

##### `QList<QGraphicsItem *> childItems() const`

返回该图元的子图元列表。

这些图元按堆叠顺序（stacking order）排序。排序顺序会综合考虑图元的**插入顺序**以及它们的 **Z 值（Z-values）**。

# Protected Functions

### 为几何信息变化做准备

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

### 处理图元状态的变化

##### `virtual QVariant itemChange(QGraphicsItem::GraphicsItemChange change, const QVariant &value)`

这个虚函数被 [[QGraphicsItem]] 调用，用于通知自定义图元其某些状态的变化。通过重写该函数，你可以==对变化做出响应==，在某些情况下（取决于 *change*）可以做调整。

*change* 表示**正在发生变化的图元属性**，*value* 是**该属性的新值**，其类型取决于具体的 *change* 。

案例：

```cpp
QVariant Component::itemChange(GraphicsItemChange change, const QVariant &value)
{
    if (change == ItemPositionChange && scene()) {
        // value is the new position.
        QPointF newPos = value.toPointF();
        QRectF rect = scene()->sceneRect();
        if (!rect.contains(newPos)) {
            // Keep the item inside the scene rect.
            newPos.setX(qMin(rect.right(), qMax(newPos.x(), rect.left())));
            newPos.setY(qMin(rect.bottom(), qMax(newPos.y(), rect.top())));
            return newPos;
        }
    }
    return QGraphicsItem::itemChange(change, value);
}
```

默认实现不做任何事，并返回 *value*。

注意：某些 [[QGraphicsItem]] 函数不能在此函数的重写中调用；详见[`GraphicsItemChange`](https://doc.qt.io/qt-6/qgraphicsitem.html#GraphicsItemChange-enum)。

### 事件处理

##### `virtual void mouseMoveEvent(QGraphicsSceneMouseEvent *event)`

该事件处理函数可被重写，用于接收该图元的鼠标移动事件。如果你收到了此事件，说明==该图元**已经接收过鼠标按下事件**了，并且当前该图元**抓住**了鼠标==。

对 *event* 调用[`QEvent::ignore()`](https://doc.qt.io/qt-6/qevent.html#ignore)或[`QEvent::accept()`](https://doc.qt.io/qt-6/qevent.html#accept)不会产生任何效果。

默认实现提供了基本的图元交互处理，例如选中和移动图元。如果你在重写此函数时仍希望保留基类的默认行为，可以在你的实现中调用`QGraphicsItem::mouseMoveEvent()`。

请注意，[`mousePressEvent()`](https://doc.qt.io/qt-6/qgraphicsitem.html#mousePressEvent)决定哪个图元要接收鼠标事件。

##### `virtual void mousePressEvent(QGraphicsSceneMouseEvent *event)`

该事件处理函数可被重写，用于接收该图元的鼠标按下事件。==只有当图元接受被按下的鼠标键时，鼠标按下事件才会传递给它==。默认情况下，图元接受所有的鼠标键，但你可通过调用[`setAcceptedMouseButtons()`](https://doc.qt.io/qt-6/qgraphicsitem.html#setAcceptedMouseButtons)来修改这一行为。

鼠标按下事件决定哪个图元将成为 mouse grabber（见[`QGraphicsScene::mouseGrabberItem()`](https://doc.qt.io/qt-6/qgraphicsscene.html#mouseGrabberItem)）。==如果你不重写该函数，则该图元不会接受鼠标事件，鼠标按下事件会传递给**该图元下方的最顶层图元**==（当然下方最顶层图元也不一定接受鼠标事件，那就继续向下传播）。

如果你重写了该函数，*event* 默认会被接受（见[`QEvent::accept()`](https://doc.qt.io/qt-6/qevent.html#accept)），且该图元将成为 mouse grabber。这使得图元可以接收将来的**移动**、**释放**和**双击**事件。如果你对 *event* 调用了[`QEvent::ignore()`](https://doc.qt.io/qt-6/qevent.html#ignore)，该图元将失去 mouse grab，*event* 会继续传递给**下方的最顶层图元**。此后不会再有其他鼠标事件传给该图元，除非收到新的鼠标按下事件。

默认实现提供了基本的图元交互处理，例如选中和移动图元。如果你在重写此函数时仍希望保留基类的默认行为，可以在你的实现中调用`QGraphicsItem::mousePressEvent()`。

对于**不可移动**且**不可选中**的图元，事件默认会被忽略（`QEvent::ignore()`）。

