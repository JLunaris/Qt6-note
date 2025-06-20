https://doc.qt.io/qt-6/qsvgrenderer.html

`QSvgRenderer`类用于将 SVG 文件的内容绘制在绘制设备上。

- Inherits：[[QObject]]
- CMake：
```cmake
find_package(Qt6 REQUIRED COMPONENTS Svg)
target_link_libraries(mytarget PRIVATE Qt6::Svg)
```

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Properties

### viewBox : QRectF

该属性保存 SVG 文档中的`viewBox`属性，该矩形指定了文档在逻辑坐标系中的可见区域。

| 访问函数     |                                     |
| -------- | ----------------------------------- |
| `QRectF` | `viewBoxF() const`                  |
| `void`   | `setViewBox(const QRect &viewbox)`  |
| `void`   | `setViewBox(const QRectF &viewbox)` |

### options : QtSvg::Options

该属性保存了一组[`QtSvg::Option`](https://doc.qt.io/qt-6/qtsvg.html#Option-enum)标志，用于启用或禁用 ==SVG 文件解析和渲染==时的各种特性。

该属性==必须在`load()`执行前设置==才能生效。注意，如果是在构造函数中传入 SVG 源参数，则在构造期间就执行加载操作，此时该属性无法生效。

| 访问函数             |                                    |
| ---------------- | ---------------------------------- |
| `QtSvg::Options` | `options() const`                  |
| `void`           | `setOptions(QtSvg::Options flags)` |

# Public Functions

### 构造和析构

##### `QSvgRenderer(QObject *parent = nullptr)`

构造一个渲染器，父对象为 *parent*。

##### `QSvgRenderer(const QString &filename, QObject *parent = nullptr)`

构造一个渲染器，父对象为 *parent*，并加载 *filename* 指代的 SVG 文件的内容。

##### `virtual ~QSvgRenderer() noexcept`

销毁该渲染器。

### 尺寸

##### `QSize defaultSize() const`

返回文件内容的默认尺寸。

### 有效性

##### `bool isValid() const`

如果当前文件是有效的，返回`true`；否则返回`false`。

# Public Slots

##### `bool load(const QString &filename)`

加载由 *filename* 指定的 SVG 文件，如果成功解析内容则返回`true`，否则返回`false`。

##### `void render(QPainter *painter)`

使用给定的 *painter* 渲染当前文档，或动画文档的当前帧。