https://doc.qt.io/qt-6/qgraphicsscene.html

`QGraphicsScene`是一个用于管理大量 2D 图形项的表面。

- Inherits：[[QObject]]

# Public Functions

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

