https://doc.qt.io/qt-6/qgraphicsview.html

`QGraphicsView`对象是一个**控件**，用于显示`QGraphicsScene`的内容。

- Inherits：[[QAbstractScrollArea]]

# Detailed Description

`QGraphicsView`将`QGraphicsScene`的内容在可滚动视口（scrollable viewport）中可视化。`QGraphicsView`是 [Graphics View Framework](https://doc.qt.io/qt-6/graphicsview.html) 的一部分。

要将场景可视化，请构造一个`QGraphicsView`对象，然后将场景的指针传递给它的构造函数。也可以稍后调用`setScene()`来设置场景。调用`QWidget::show()`后，视图默认会==滚动到场景的中心==，并显示当前可见的所有图元。例如：

```cpp
QGraphicsScene scene;
scene.addText("Hello, world!");

QGraphicsView view(&scene);
view.show();
```

你可以通过滚动条显式滚动到场景中的任意位置，或者调用[`centerOn()`](https://doc.qt.io/qt-6/qgraphicsview.html#centerOn)。传递一个点给[`centerOn()`](https://doc.qt.io/qt-6/qgraphicsview.html#centerOn)，`QGraphicsView`会滚动其视口**使该点位于视口中心**；传递一个图元给[`centerOn()`](https://doc.qt.io/qt-6/qgraphicsview.html#centerOn)，`QGraphicsView`会滚动其视口**使该图元的中心点位于视口中心**。如果你只想保证某个区域可见（但不必须居中），则调用[`ensureVisible()`](https://doc.qt.io/qt-6/qgraphicsview.html#ensureVisible)。

`QGraphicsView`可用于显示整个场景，或其中一部分。默认情况下，视图首次显示时会自动检测可视区域（通过调用 `QGraphicsScene::itemsBoundingRect()`）。如果你想自己设置可视区域矩形，可以调用[`setSceneRect()`](https://doc.qt.io/qt-6/qgraphicsview.html#sceneRect-prop)，这会适当调整滚动条的范围。注意，虽然场景的尺寸可以几乎无限大，但滚动条的范围永远不会超出整数范围（`INT_MIN`，`INT_MAX`）。


# Properties

### sceneRect : QRectF

该属性保存该视图查看的场景的区域。

场景矩形定义了场景的范围，对视图而言，表示你可以使用滚动条导航的场景区域。

==如果未设置，或设为空矩形，则该属性的值与`QGraphicsScene::sceneRect()`相同，并且会随`QGraphicsScene::sceneRect()`的改变而改变==。否则，视图的场景矩形不受场景影响。

注意，虽然场景的尺寸可以几乎无限大，但滚动条的范围永远不会超出整数范围（`INT_MIN`，`INT_MAX`）。当场景大于滚动条的值时，你可以选择使用[`translate()`](https://doc.qt.io/qt-6/qgraphicsview.html#translate)来导航场景。

该属性的默认值：位于原点、宽高为`0`的矩形。

| 访问函数     |                                                    |
| -------- | -------------------------------------------------- |
| `QRectF` | `sceneRect() const`                                |
| `void`   | `setSceneRect(const QRectF &rect)`                 |
| `void`   | `setSceneRect(qreal x, qreal y, qreal w, qreal h)` |

### backgroundBrush : QBrush

该属性保存场景的背景画刷。

此属性为该视图中的场景设置背景画刷。它将==覆盖场景自身的背景画刷设置==，并定义[`drawBackground()`](https://doc.qt.io/qt-6/qgraphicsview.html#drawBackground)的行为。要为该视图提供**自定义的背景绘制**，可以重写[`drawBackground()`](https://doc.qt.io/qt-6/qgraphicsview.html#drawBackground)。

默认的背景画刷为`Qt::NoBrush`。

| 访问函数     |                                           |
| -------- | ----------------------------------------- |
| `QBrush` | `backgroundBrush() const`                 |
| `void`   | `setBackgroundBrush(const QBrush &brush)` |

### foregroundBrush : QBrush

该属性保存场景的前景画刷。

此属性为该视图中的场景设置前景画刷。它将==覆盖场景自身的前景画刷设置==，并定义[`drawForeground()`](https://doc.qt.io/qt-6/qgraphicsview.html#drawForeground)的行为。要为该视图提供**自定义的前景绘制**，可以重写[`drawForeground()`](https://doc.qt.io/qt-6/qgraphicsview.html#drawForeground)。

默认的前景画刷为`Qt::NoBrush`。

| 访问函数     |                                           |
| -------- | ----------------------------------------- |
| `QBrush` | `foregroundBrush() const`                 |
| `void`   | `setForegroundBrush(const QBrush &brush)` |

### transformationAnchor : ViewportAnchor

在变换期间，视图该如何定位场景。

视图的**变换矩阵改变时**或视图的**坐标系被变换时**，`QGraphicsView`使用该属性来决定在视口中如何定位场景。默认行为——[`AnchorViewCenter`](https://doc.qt.io/qt-6/qgraphicsview.html#ViewportAnchor-enum)确保==**视图中心对应的场景点**在变换期间保持不动==（例如，当旋转时，场景会围绕视图的中心旋转）。

注意，==仅在**场景的一部分可见**时（即出现滚动条时）该属性才生效。否则，如果整个场景都能显示在视图中，`QGraphicsView`会使用视图的对齐属性[`alignment`](https://doc.qt.io/qt-6/qgraphicsview.html#alignment-prop)来定位场景在视图中的位置==。

相关内容：[`QGraphicsView::ViewportAnchor`](https://doc.qt.io/qt-6/qgraphicsview.html#ViewportAnchor-enum)

| 访问函数                            |                                                                 |
| ------------------------------- | --------------------------------------------------------------- |
| `QGraphicsView::ViewportAnchor` | `transformationAnchor() const`                                  |
| `void`                          | `setTransformationAnchor(QGraphicsView::ViewportAnchor anchor)` |

# Public Functions

### 构造和析构

##### `QGraphicsView(QWidget *parent = nullptr)`

构造一个`QGraphicsView`。*parent* 被传递给`QWidget`的构造函数。

##### `QGraphicsView(QGraphicsScene *scene, QWidget *parent = nullptr)`

构造一个`QGraphicsView`，并将其显示场景设为 *scene*。*parent* 被传递给`QWidget`的构造函数。

##### `virtual ~QGraphicsView() noexcept`

析构函数。

### 场景

##### `void setScene(QGraphicsScene *scene)`

将当前场景设为 *scene*。如果 *scene* 已经被视图查看，则函数不执行任何操作。

当把场景设置到视图上时，[`QGraphicsScene::changed()`](https://doc.qt.io/qt-6/qgraphicsscene.html#changed)信号会自动连接到视图的`updateScene()`槽上，并且视图的滚动条会根据场景的大小自动调整。

==视图不会接管 *scene* 的所有权==。

##### `QGraphicsScene *scene() const`

返回当前视图正在查看的场景。如果当前没有查看任何场景，则返回`nullptr`。

### 变换

##### `QTransform transform() const`

返回视图当前的变换矩阵。如果没有设置变换，则返回单位矩阵。

##### `void scale(qreal sx, qreal sy)`

按比例`(sx, sy)`缩放当前视图的变换。

### 映射

##### `QPointF mapToScene(const QPoint &point) const`

将视口坐标 *point* 映射到场景坐标。

注意：映射 ***point* 处像素覆盖的整个矩形**而不是**点本身**会更有用。为此，可以调用`mapToScene(QRect(point, QSize(2, 2)))`。

### 渲染

##### `void render( 参数列表 )`

```cpp
void render(
	QPainter *painter,
	const QRectF &target = QRectF(),
	const QRect &source = QRect(),
	Qt::AspectRatioMode aspectRatioMode = Qt::KeepAspectRatio
)
```

使用 *painter*，将==**视图坐标系**中的 *source* 矩形==对应的场景内容渲染到==**绘制设备坐标系**中的 *target* 矩形==。该函数常用于捕获视图的内容到一个绘制设备上，例如`QImage`（即获取截图），或用于`QPrinter`打印。例如：

```cpp
QGraphicsScene scene;
scene.addItem(...
...

QGraphicsView view(&scene);
view.show();
...

QPrinter printer(QPrinter::HighResolution);
printer.setPageSize(QPrinter::A4);
QPainter painter(&printer);

// print, fitting the viewport contents into a full page
view.render(&painter);

// print the upper half of the viewport into the lower.
// half of the page.
QRect viewport = view.viewport()->rect();
view.render(&painter,
			QRectF(0, printer.height()/2, printer.width(). printer.height()/2),
			viewport.adjusted(0, 0, 0, -viewport.height()/2));
```

如果 *source* 是空矩形，则使用`viewport()->rect()`确定渲染区域。如果 *target* 是空矩形，则使用 *painter* 的绘制设备的尺寸。

*source* 矩形内容将根据 *aspectRatioMode* 进行转换，以适应 *target* 矩形。默认情况下，纵横比保持不变，*source* 被缩放以适应 *target* 。

# Public Slots

##### `void updateScene(const QList<QRectF> &rects)`

安排（schedule）对场景中矩形区域 *rects* 的更新。

# Protected Functions

##### `virtual void drawBackground(QPainter *painter, const QRectF &rect)`

使用 *painter* 画**场景**的**背景**（在任何图元和前景被画前）。==重写该函数来为该视图定制背景==。

如果你只是想为背景定义颜色、纹理或渐变，可以直接调用`setBackgroundBrush()`，无需重写该函数。

所有绘制都是在**场景坐标系**（scene coordinates）中做的。*rect* 指的是**场景中暴露到视图的矩形区域**，它基于**场景坐标系**。

默认实现是使用**视图**的 backgroundBrush 填充 *rect* 。==如果未设置该画刷（即，使用默认值`Qt::NoBrush`），则调用**场景**的`drawBackground()`==。

##### `virtual void drawForeground(QPainter *painter, const QRectF &rect)`

使用 *painter* 画**场景**的**前景**（在背景和所有图元画完后）。==重写该函数来为该视图定制前景==。

如果你只是想为前景定义颜色、纹理或渐变，可以直接调用`setForegroundBrush()`，无需重写该函数。

所有绘制都是在**场景坐标系**（scene coordinates）中做的。*rect* 指的是**场景中暴露到视图的矩形区域**，它基于**场景坐标系**。

默认实现是使用**视图**的 foregroundBrush 填充 *rect* 。==如果未设置该画刷（即，使用默认值`Qt::NoBrush`），则调用**场景**的`drawForeground()`==。

