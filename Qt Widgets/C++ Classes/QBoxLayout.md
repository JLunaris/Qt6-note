https://doc.qt.io/qt-6/qboxlayout.html

`QBoxLayout`类将子控件水平地或垂直地排列。

- Inherits：[[QLayout]]
- Inherited By：[QHBoxLayout](https://doc.qt.io/qt-6/qhboxlayout.html)、[QVBoxLayout](https://doc.qt.io/qt-6/qvboxlayout.html)

# Public Functions

##### `void addSpacing(int size)`

向布局末尾添加一个==不可拉伸的==间距（[`QSpacerItem`](https://doc.qt.io/qt-6/qspaceritem.html)），间距大小为 *size*。

`QBoxLayout`已提供默认的 margin 和 spacing，该函数用于额外添加指定大小的间距。

