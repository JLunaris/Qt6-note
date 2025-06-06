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