https://doc.qt.io/qt-6/qgraphicssvgitem.html

`QGraphicsSvgItem`类是一个可用于渲染 SVG 文件内容的==`QGraphicsItem`==。

- Inherits：[[QGraphicsObject]]
- CMake：
```cmake
find_package(Qt6 REQUIRED COMPONENTS SvgWidgets)
target_link_libraries(mytarget PRIVATE Qt6::SvgWidgets)
```

# Public Functions

### 构造

##### `QGraphicsSvgItem(QGraphicsItem *parent = nullptr)`

构造一个 SVG 图元，父图元为 *parent*。

##### `QGraphicsSvgItem(const QString &fileName, QGraphicsItem *parent = nullptr)`

构造一个 SVG 图元，父图元为 *parent*，并加载 *filename* 指代的 SVG 文件的内容。