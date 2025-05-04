https://doc.qt.io/qt-6/qimage.html

`QImage`是一种独立于硬件的图像表示，允许直接访问像素数据，并可用作**绘制设备**。

- Inherits：`QPaintDevice`

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Public Functions

##### `void setDevicePixelRatio(qreal scaleFactor)`

设置图像的**设备像素比**（device pixel ratio），即**图像像素**与**设备无关像素**的比值。

默认的`scaleFactor`为`1.0`。将其设为其他值会产生两个影响：

1. 此图像的`QPainter`将会被缩放。例如，在一个 200×200 的图像上绘制，如果比值为`2.0`，则有效的（设备无关的）绘制范围为 100×100。
2. 根据图像尺寸计算布局几何信息的代码路径将考虑该比值：`QSize layoutSize = image.size() / image.devicePixelRatio()` 这样做的最终效果是图像显示为**高 DPI 图像**而非一张大的图像（见 [Drawing High Resolution Versions of Pixmaps and Images](https://doc.qt.io/qt-6/qpainter.html#drawing-high-resolution-versions-of-pixmaps-and-images)）。