https://doc.qt.io/qt-6/qpixmap.html

`QPixmap`是一种屏幕外的图像表示，可用作**绘制设备**（paint device）。

- Inherits：`QPaintDevice`
- Inherited By：`QBitmap`

# Detailed Description

Qt 提供了4个处理图像数据的类：

- `QImage`：设计并优化用于 ==I/O== 和==对像素直接访问和操作==
- `QPixmap`：设计并优化用于==在屏幕上显示图像==
- `QBitmap`：只是一个继承自`QPixmap`的方便类——它确保**图像深度为1**
- `QPicture`：是一个用于==记录并重放`QPainter`命令==的**绘制设备**（paint device）

如果一个`QPixmap`对象真的是位图，则`isQBitmap()`函数返回`true`，否则返回`false`。

##### 显示在屏幕上

使用`QLabel`或`QAbstractButton`的子类（如`QPushButton`和`QToolButton`）可以将`QPixmap`很容易地显示在屏幕上——通过`QLabel`的 [pixmap](https://doc.qt.io/qt-6/qlabel.html#pixmap-prop) 属性、`QAbstractButton`的 [icon](https://doc.qt.io/qt-6/qabstractbutton.html#icon-prop) 属性。

##### 数据传递

`QPixmap`对象可以**按值传递**，因为`QPixmap`类使用[隐式数据共享](https://doc.qt.io/qt-6/implicit-sharing.html)。`QPixmap`对象还支持**流处理**。

##### 像素管理（绘制）

pixmap 中的像素数据是内部的（由底层窗口系统管理）。因为`QPixmap`是`QPaintDevice`的子类，所以==可以使用`QPainter`直接在 pixmap 上绘制==。==只能通过`QPainter`函数或将`QPixmap`转为`QImage`来访问像素==；但是，`fill()`函数可用于将整个 pixmap 初始化为给定的颜色。

Qt 提供了`QImage`和`QPixmap`相互转换的函数。典型的案例是：使用`QImage`类载入图像文件，然后可以对图像数据进行处理，最后将`QImage`对象转为`QPixmap`对象从而可以显示在屏幕上。或者，若无修改图像的需求，可以直接将图像文件载入到`QPixmap`对象中。

##### 图像文件读写

`QPixmap`提供了几种**读取**图像文件的方法：可以在构造`QPixmap`对象时载入文件，也可以在之后使用`load()`或`loadFromData()`函数。载入图像时，文件名引用的可以是**磁盘上的实际文件**，也可以是**应用程序的嵌入资源**。有关如何向应用程序的可执行文件中嵌入图片和其他资源文件，详见 [The Qt Resource System](https://doc.qt.io/qt-6/resources.html)。

只需调用`save()`函数即可保存`QPixmap`对象。

Qt 支持的图像格式可通过[`QImageReader::supportedImageFormats()`](https://doc.qt.io/qt-6/qimagereader.html#supportedImageFormats)和[`QImageWriter::supportedImageFormats()`](https://doc.qt.io/qt-6/qimagewriter.html#supportedImageFormats)获取。新的文件格式可作为插件添加。Qt 默认支持以下格式：

| 格式   | 说明                                    | Qt的支持 |
| ---- | ------------------------------------- | ----- |
| BMP  | Windows Bitmap                        | 读/写   |
| GIF  | Graphic Interchange Format (optional) | 读     |
| JPG  | Joint Photographic Experts Group      | 读/写   |
| JPEG | Joint Photographic Experts Group      | 读/写   |
| PNG  | Portable Network Graphics             | 读/写   |
| PBM  | Portable Bitmap                       | 读     |
| PGM  | Portable Graymap                      | 读     |
| PPM  | Portable Pixmap                       | 读/写   |
| XBM  | X11 Bitmap                            | 读/写   |
| XPM  | X11 Pixmap                            | 读/写   |
##### Pixmap 信息

`QPixmap`提供了一组函数，用于获取有关 pixmap 的各种信息：

|                       | 函数                                                                                                                                                                                                                                                                                        |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Geometry              | `size()`、`width()`、`height()`：提供 pixmap 的尺寸信息<br>`rect()`：返回图像的包围矩形                                                                                                                                                                                                                       |
| Alpha component       | `hasAlphaChannel()`：若 pixmap 具有与 Alpha 通道相关的格式，则返回`true`，否则返回`false`。<br>`hasAlpha()`、`setMask()`、`mask()`：是遗留函数，不应使用。它们可能非常慢。<br>`createHeuristicMask()`：为 pixmap 创建一个1-bpp启发式掩码（即 `QBitmap`），返回该`QBitmap`。<br>`createMaskFromColor()`：基于给定的颜色，为 pixmap 创建一个掩码（即 `QBitmap`，返回该`QBitmap`。 |
| Low-level information | `depth()`：返回 pixmap 的深度<br>`defaultDepth()`：返回默认深度，即应用程序在给定屏幕上使用的深度<br>`cacheKey()`：返回唯一标识`QPixmap`对象的内容的数字                                                                                                                                                                               |
##### Pixmap 转换

使用`toImage()`将`QPixmap`转换为`QImage`。同样，使用`fromImage()`将`QImage`转换为`QPixmap`。如果这种转换开销过大，可以改用`QBitmap::fromImage()`。

`QPixmap`与`HICON`的相互转换：先将`QPixmap`转为`QImage`，然后使用`QImage::toHICON()`和`QImage::fromHICON()`。

##### Pixmap 变换

`QPixmap`支持许多函数来创建原始图像的变换版本：

`scaled()`、`scaledToWidth()`、`scaledToHeight()`函数返回缩放后的 pixmap 副本，`copy()`函数创建原始图像的深拷贝副本。

`transformed()`函数返回**使用给定变换矩阵和变换模式**变换后的 pixmap 副本：在内部会自动调整变换矩阵，以抵消不希望的平移，也就是说，`transformed()`返回的是能够包含原始 pixmap 所有经变换后的点的**最小 pixmap**。

`trueMatrix()`返回用于变换 pixmap 的实际矩阵。

# Public Functions

##### `QPixmap copy(const QRect &rectangle = QRect()) const`

返回由`rectangle`指定的 pixmap 子区域的**深拷贝**。

如果`rectangle`为空，则会拷贝整个图像。

##### `QPixmap copy(int x, int y, int width, int height) const`

等价于`copy(QRect(x, y, width, height))`。