https://doc.qt.io/qt-6/qgraphicssvgitem.html

`QGraphicsSvgItem`类是一个可用于渲染 SVG 文件内容的==`QGraphicsItem`==。

- Inherits：[[QGraphicsObject]]
- CMake：
```cmake
find_package(Qt6 REQUIRED COMPONENTS SvgWidgets)
target_link_libraries(mytarget PRIVATE Qt6::SvgWidgets)
```

# Detailed Description

`QGraphicsSvgItem`提供了一种将 SVG 文件渲染到`QGraphicsView`的方式。可通过将想要渲染的 SVG 文件的路径传递给构造函数来创建`QGraphicsSvgItem`，或在之后为它显式设置一个共享的`QSvgRenderer`。

注意，将`QSvgRenderer`设置到`QGraphicsSvgItem`上不会使该图元接管其所有权，因此如果使用`setSharedRenderer()`方法，必须确保`QSvgRenderer`对象的生命周期至少与`QGaraphicsSvgItem`一样长。

`QGraphicsSvgItem`支持通过`setElementId()`方法渲染 SVG 文件的**部分内容**。如果调用了`setElementId()`，则只会渲染指定 ID 的 SVG 元素（及其子元素）。这为有选择地渲染包含大量离散元素的大型 SVG 文件提供了一种便捷方式。例如下列代码从一个包含所有扑克牌的 SVG 文件中只渲染”大小王“牌：

```cpp
QSvgRenderer *renderer = new QSvgRenderer(QLatin1String("SvgCardDeck.svg"));
QGraphicsSvgItem *black = new QGraphicsSvgItem();
QGraphicsSvgItem *red   = new QGraphicsSvgItem();

black->setSharedRenderer(renderer);
black->setElementId(QLatin1String("black_joker"));

red->setSharedRenderer(renderer);
red->setElementId(QLatin1String("red_joker"));
```

可通过直接操纵图元的变换矩阵来设置图元的尺寸。

默认情况下，SVG 渲染会被缓存（使用[`QGraphicsItem::DeviceCoordinateCache`](https://doc.qt.io/qt-6/qgraphicsitem.html#CacheMode-enum)模式）以加速图元显示。将[`QGraphicsItem::NoCache`](https://doc.qt.io/qt-6/qgraphicsitem.html#CacheMode-enum)传递给[`QGraphicsItem::setCacheMode()`](https://doc.qt.io/qt-6/qgraphicsitem.html#setCacheMode)可以禁用缓存。

# Properties

### elementId : QString

该属性保存 SVG 元素的 XML ID。

| 访问函数      |                                   |
| --------- | --------------------------------- |
| `QString` | `elementId() const`               |
| `void`    | `setElementId(const QString &id)` |

# Public Functions

### 构造

##### `QGraphicsSvgItem(QGraphicsItem *parent = nullptr)`

构造一个 SVG 图元，父图元为 *parent*。

##### `QGraphicsSvgItem(const QString &fileName, QGraphicsItem *parent = nullptr)`

构造一个 SVG 图元，父图元为 *parent*，并加载 *filename* 指代的 SVG 文件的内容。

### 渲染器

##### `QSvgRenderer *renderer() const`

返回图元当前使用的`QSvgRenderer`。

### 设置共享渲染器

##### `void setSharedRenderer(QSvgRenderer *renderer)`

将 *renderer* 设为该图元的共享渲染器。

通过使用此方法，可在多个图元上共享同一个`QSvgRenderer`。这意味着 **SVG 文件只会被解析一次**。传递给此方法的`QSvgRenderer`必须在图元使用它的期间一直存在。

### XML元素的ID

##### `void setElementId(const QString &id)`

将元素的 XML ID 设为 *id*。

##### `QString elementId() const`

返回当前正在渲染的元素的 XML ID。如果当前渲染的是整个文件，则返回空字符串。

