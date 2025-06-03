https://doc.qt.io/qt-6/qgraphicsscene.html

`QGraphicsScene`是一个用于管理大量 2D 图形项的表面。

- Inherits：[[QObject]]

# Detailed Description

场景的 **边界矩形（bounding rect）** 由`setSceneRect()`设置。图元可以放置在场景中的任意位置，并且**默认情况下场景的尺寸是无限制的**。场景矩形仅用于内部记录，维护场景的图元索引。==如果未设置场景矩形，`QGraphicsScene`将使用所有图元的边界区域（即`itemsBoundingRect()`）作为场景矩形==。然而，`itemsBoundingRect()`是一个开销较大的函数，因为它需要收集场景中每个图元的位置数据。因此，在操作大型场景时，**应始终手动设置场景矩形**以提高性能。

# Properties

### sceneRect : QRectF

该属性保存场景矩形，即场景的边界矩形。

场景矩形定义了场景的范围。它主要用于以下两点：

1. `QGraphicsView` 使用它来确定视图的默认可滚动区域
2. `QGraphicsScene` 使用它来管理图元索引

如果未设置，或设为空矩形，`sceneRect()`将返回自场景创建以来所有图元的最大边界矩形（即一个在场景中添加或移动图元时会增大，但从不会缩小的矩形）。

| 访问函数     |                                                    |
| -------- | -------------------------------------------------- |
| `QRectF` | `sceneRect() const`                                |
| `void`   | `setSceneRect(const QRectF &rect)`                 |
| `void`   | `setSceneRect(qreal x, qreal y, qreal w, qreal h)` |

### backgroundBrush : QBrush

该属性保存场景的背景画刷。

修改该属性来改变场景背景的颜色、纹理或渐变。默认的背景画刷为`Qt::NoBrush`。背景在所有图元绘制前绘制（即画在所有图元下方）。

案例：

```cpp
QGraphicsScene scene;
QGraphicsView view(&scene);
view.show();

// a blue background
scene.setBackgroundBrush(Qt::blue);

// a gradient background
QRadialGradient gradient(0, 0, 10);
gradient.setSpread(QGradient::RepeatSpread);
scene.setBackgroundBrush(gradient);
```

[`QGraphicsScene::render()`](https://doc.qt.io/qt-6/qgraphicsscene.html#render)会调用`drawBackground()`画场景背景。

如果需要对背景绘制进行更精细的控制，可以在`QGraphicsScene`的派生类中重写`drawBackground()`。

| 访问函数     |                                           |
| -------- | ----------------------------------------- |
| `QBrush` | `backgroundBrush() const`                 |
| `void`   | `setBackgroundBrush(const QBrush &brush)` |

### foregroundBrush : QBrush

该属性保存场景的前景画刷。

修改该属性来改变场景前景的颜色、纹理或渐变。默认的前景画刷为`Qt::NoBrush`。前景在所有图元绘制完成后绘制（即画在所有图元上方）。

案例：

```cpp
QGraphicsScene scene;
QGraphicsView view(&scene);
view.show();

// a white semi-transparent foreground
scene.setForegroundBrush(QColor(255, 255, 255, 127));

// a grid foreground
scene.setForegroundBrush(QBrush(Qt::lightGray, Qt::CrossPattern));
```

[`QGraphicsScene::render()`](https://doc.qt.io/qt-6/qgraphicsscene.html#render)会调用`drawForeground()`画场景前景。

如果需要对前景绘制进行更精细的控制，可以在`QGraphicsScene`的派生类中重写`drawForeground()`。

| 访问函数     |                                           |
| -------- | ----------------------------------------- |
| `QBrush` | `foregroundBrush() const`                 |
| `void`   | `setForegroundBrush(const QBrush &brush)` |

# Public Functions

### 构造和析构

##### `QGraphicsScene(QObject *parent = nullptr)`

构造一个`QGraphicsScene`对象。*parent* 被传递给`QWidget`的构造函数。

##### `QGraphicsScene(const QRectF &sceneRect, QObject *parent = nullptr)`

构造一个`QGraphicsScene`对象，使用 *sceneRect* 作为其场景矩形。*parent* 被传递给`QWidget`的构造函数。

##### `QGraphicsScene(qreal x, qreal y, qreal width, qreal height, QObject *parent = nullptr)`

等价于`QGraphicsScene(QRectF {x, y, width, height}, parent)`。*parent* 被传递给`QWidget`的构造函数。

##### `virtual ~QGraphicsScene() noexcept`

销毁场景对象前，先从该场景中**移除并删除所有图元**。

此外，该场景对象会从应用程序的**全局场景列表**中移除，并且会从所有**与之关联的视图**中移除。

### 渲染
##### `void render( 参数列表 )`

```Cpp
void render(
	QPainter *painter,
	const QRectF &target = QRectF(),
	const QRectF &source = QRectF(),
	Qt::AspectRatioMode aspectRatioMode = Qt::KeepAspectRatio
)
```

使用 *painter*，将场景中的 *source* 矩形渲染到 *target* 矩形。该函数常用于捕获场景的内容到一个绘制设备上，例如`QImage`（即获取截图），或用于`QPrinter`打印。例如：

```cpp
QGraphicsScene scene;
scene.addItem(...
...
QPrinter printer(QPrinter::HighResolution);
printer.setPaperSize(QPrinter::A4);

QPainter painter(&printer);
scene.render(&painter);
```

如果 *source* 是空矩形，则使用`sceneRect()`确定渲染区域。如果 *target* 是空矩形，则使用 *painter* 的绘制设备的尺寸。

*source* 矩形内容将根据 *aspectRatioMode* 进行转换，以适应 *target* 矩形。默认情况下，纵横比保持不变，*source* 被缩放以适应 *target* 。

# Protected Functions

### 前景和背景绘制
##### `virtual void drawBackground(QPainter *painter, const QRectF &rect)`

使用 *painter* 画场景的**背景**（在任何图元和前景被画前）。==重写该函数来为场景定制背景==。

所有绘制都是在**场景坐标系**（scene coordinates）中做的。*rect* 指的是**场景中暴露到视图的矩形区域**，它基于**场景坐标系**。

如果你只是想为背景定义颜色、纹理或渐变，可以直接调用`setBackgroundBrush()`，无需重写该函数。

##### `virtual void drawForeground(QPainter *painter, const QRectF &rect)`

使用 *painter* 画场景的**前景**（在背景和所有图元画完后）。==重写该函数来为场景定制前景==。

所有绘制都是在**场景坐标系**（scene coordinates）中做的。*rect* 指的是**场景中暴露到视图的矩形区域**，它基于**场景坐标系**。

如果你只是想为前景定义颜色、纹理或渐变，可以直接调用`setForegroundBrush()`，无需重写该函数。

### 事件处理

##### `virtual void mousePressEvent(QGraphicsSceneMouseEvent *mouseEvent)`

用于处理事件 *mouseEvent*，可在派生类中重写以接收场景中的鼠标按下事件。

默认实现取决于场景的状态：

- 如果当前存在 mouse grabber 图元，则事件会发送给该图元；
- 否则，它会被转发给在事件的场景位置上的、接受鼠标事件的、最顶层的可见图元，然后该图元会立即成为 mouse grabber 图元。

如果场景中给定位置上没有任何图元，则：

- 选区（selection area）会被重置；
- 任何焦点图元（focus item）失去其输入焦点；
- 然后该事件会被忽略。

注意：哪些图元会被该函数视为“可见的”，见[`items()`](https://doc.qt.io/qt-6/qgraphicsscene.html#items)。

