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

### Pixmap 变换

##### `QPixmap copy(const QRect &rectangle = QRect()) const`

返回由`rectangle`指定的 pixmap 子区域的**深拷贝**。

如果`rectangle`为空，则会拷贝整个图像。

##### `QPixmap copy(int x, int y, int width, int height) const`

等价于`copy(QRect(x, y, width, height))`。

### 设备像素比相关
##### `void setDevicePixelRatio(qreal scaleFactor)`

设置图像的**设备像素比**（device pixel ratio），即**设备像素**与**设备无关像素**的比值。

默认的`scaleFactor`为`1.0`。将其设为其他值会产生两个影响：

1. ==使用此图像创建的`QPainter`将会被缩放==。例如，在一个 200×200 的图像上绘制，如果比值为`2.0`，则有效的（设备无关的）绘制范围为 100×100。
2. 根据图像尺寸计算**布局**几何信息的代码路径将考虑该比值，处理为 `QSize layoutSize = pixmap.size() / pixmap.devicePixelRatio()`，这样做的最终效果是图像==显示为**高 DPI 图像**而不是大图像==（见 [Drawing High Resolution Versions of Pixmaps and Images](https://doc.qt.io/qt-6/qpainter.html#drawing-high-resolution-versions-of-pixmaps-and-images)）。

测试代码：在设备像素比为 1.5 的情况下，执行下列代码，观察 pixmap 的设备像素比为 1 和 1.5 时的不同。
```cpp
#include <QApplication>  
#include <QMainWindow>  
#include <print>  
#include <QScreen>  
#include <QLabel>  
  
int main(int argc, char *argv[])  
{  
    QApplication app {argc, argv};  
    QMainWindow win;  
      
    QScreen *screen {QGuiApplication::primaryScreen()};  
    QPixmap pixmap {screen->grabWindow()};  // 捕获当前屏幕
    std::println("pixmap的设备像素比: {}", pixmap.devicePixelRatio());  
    std::println("pixmap的大小: ({}, {})", pixmap.width(), pixmap.height());  
  
    pixmap.setDevicePixelRatio(1); // 注释掉该行，试一下  
  
    QLabel *label {new QLabel {&win}};  
    label->setPixmap(pixmap);  
    label->resize(pixmap.size());  
      
    win.show();  
    return QApplication::exec();  
}
```

##### `qreal devicePixelRatio() const`

返回图像的**设备像素比**（device pixel ratio），即**设备像素**与**设备无关像素**的比值。

根据图像尺寸计算**布局**几何信息时使用此函数：`QSize layoutSize = image.size() / image.devicePixelRatio()`。

默认值为`1.0`。

##### `QSizeF deviceIndependentSize() const`

返回图像的尺寸，以设备无关像素为单位。

用户界面尺寸计算中获取图像尺寸时，应使用该函数。

数值上等价于==`pixmap.size() / pixmap.devicePixelRatio()`==。

