https://doc.qt.io/qt-6/qgraphicsview.html

`QGraphicsView`对象是一个**控件**，用于显示`QGraphicsScene`的内容。

- Inherits：[[QAbstractScrollArea]]

# Public Functions

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