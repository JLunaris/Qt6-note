https://doc.qt.io/qt-6/qpixmap.html

`QPixmap`类是一个屏幕外的图像表示，可用作**绘制设备**（paint device）。

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

pixmap 中的像素数据是内部的（由底层窗口系统管理）。因为`QPixmap`是`QPaintDevice`的子类，所以==可以使用`QPainter`直接在 pixmap 上绘制==。只能通过`QPainter`函数或将`QPixmap`转为`QImage`来==访问像素==；但是，`fill()`函数可用于将整个 pixmap 初始化为给定的颜色。

Qt 提供了`QImage`和`QPixmap`相互转换的函数。典型的案例是：使用`QImage`类载入图像文件，然后可以对图像数据进行处理，最后将`QImage`对象转为`QPixmap`对象从而可以显示在屏幕上。或者，若无修改图像的需求，可以直接将图像文件载入到`QPixmap`对象中。

##### 其他功能

`QPixmap`提供了一组函数，用于获取有关 pixmap 的各种信息。此外，还有几个函数支持 pixmap 的变换（缩放/旋转/裁剪等）。

# Public Functions

##### `QPixmap copy(const QRect &rectangle = QRect()) const`

返回由`rectangle`指定的 pixmap 子区域的**深拷贝**。

如果`rectangle`为空，则会拷贝整个图像。

##### `QPixmap copy(int x, int y, int width, int height) const`

等价于`copy(QRect(x, y, width, height))`。