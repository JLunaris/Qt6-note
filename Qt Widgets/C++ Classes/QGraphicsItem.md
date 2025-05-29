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

