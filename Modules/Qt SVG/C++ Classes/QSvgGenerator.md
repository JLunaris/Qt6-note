https://doc.qt.io/qt-6/qsvggenerator.html

`QSvgGenerator`是一个用于==创建 SVG 图形==的绘制设备。

- Inherits：[[QPaintDevice]]
- CMake：
```cmake
find_package(Qt6 REQUIRED COMPONENTS Svg)
target_link_libraries(mytarget PRIVATE Qt6::Svg)
```

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Properties

### outputDevice : QIODevice *

该属性保存**输出设备**（即生成的 SVG 应输出到哪个设备）。

如果同时指定了输出设备和文件名，则输出设备具有优先权。

| 访问函数          |                                            |
| ------------- | ------------------------------------------ |
| `QIODevice *` | `outputDevice() const`                     |
| `void`        | `setOutputDevice(QIODevice *outputDevice)` |

### fileName : QString

该属性保存**目标文件名**（即生成的 SVG 应输出到哪个文件）。

| 访问函数      |                                        |
| --------- | -------------------------------------- |
| `QString` | `fileName() const`                     |
| `void`    | `setFileName(const QString &fileName)` |

### viewBox : QRectF

该属性保存生成的 SVG 图像的**视图框**（viewBox）。

默认情况下，该属性被设置为`QRect(0, 0, -1, -1)`，表示生成器不应输出`<svg>`元素的 viewBox 属性。

> 注意：`QPainter`在生成器上激活（active）时，无法更改此属性。

| 访问函数     |                                     |
| -------- | ----------------------------------- |
| `QRectF` | `viewBoxF() const`                  |
| `void`   | `setViewBox(const QRect &viewBox)`  |
| `void`   | `setViewBox(const QRectF &viewBox)` |

### size : QSize

该属性保存生成的 SVG 图像的**尺寸**（size）。

默认情况下，该属性被设置为`QSize(-1, -1)`，表示生成器不应输出`<svg>`元素的 size 属性。

> 注意：`QPainter`在生成器上激活（active）时，无法更改此属性。

| 访问函数    |                              |
| ------- | ---------------------------- |
| `QSize` | `size() const`               |
| `void`  | `setSize(const QSize &size)` |

# Detailed Description

该绘制设备表示 **可缩放矢量图形（SVG）** 绘图。类似于`QPrinter`，它被设计为一个生成特定格式输出的**只写设备**。

要写 SVG 文件，首先需要设置`fileName`或`outputDevice`属性来配置输出。通常需要设置`size`属性来指定绘图尺寸；如果该图形将被包含在另一个图形中，还需要设置`viewBox`属性。

```cpp
QSvgGenerator generator;
generator.setFileName(path);
generator.setSize(QSize(200, 200));
generator.setViewBox(QRect(0, 0, 200, 200));
generator.setTitle(tr("SVG Generator Example Drawing"));
generator.setDescription(tr("An SVG drawing created by the SVG Generator "
                            "Example provided with Qt."));
```

其他元数据可通过设置`title`、`description`、`resolution`属性指定。

与其他`QPaintDevice`的派生类一样，可使用`QPainter`对象在这种类的实例上绘制：

```cpp
QPainter painter;
painter.begin(&generator);
...
painter.end();
```

绘制的执行方式与在其他绘制设备上相同。不过，需要使用`QPainter::begin()`和`end()`来显式开始和结束绘制。

# Public Functions

### 构造和析构

##### `QSvgGenerator()`

构造一个新生成器，使用 ==SVG Tiny 1.2 规范==。

##### `virtual ~QSvgGenerator() noexcept`

销毁该生成器。

