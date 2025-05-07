https://doc.qt.io/qt-6/qabstractscrollarea.html

- Inherits：[[QFrame]]
- Inherited By：[[QGraphicsView]]，[`QScrollArea`](https://doc.qt.io/qt-6/qscrollarea.html) 等

# Detailed Description

`QAbstractScrollArea`是**滚动区域**（scrolling area）的底层抽象。它提供了一个名为**视口**（viewport）的中心控件，区域的内容将在其中滚动（即，内容的可见部分会在视口中渲染）。

视口的右侧为**垂直滚动条**，下侧为**水平滚动条**。当所有区域内容都能完全显示在视口内时，每个滚动条都可以是可见的/隐藏的，取决于滚动条的[`Qt::ScrollBarPolicy`](https://doc.qt.io/qt-6/qt.html#ScrollBarPolicy-enum)。滚动条隐藏时，视口会扩展以覆盖所有可用空间；当滚动条再次变为可见时，视口会收缩以给滚动条腾出空间。

可以在视口周围保留一块 margin 区域，见[`setViewportMargins()`](https://doc.qt.io/qt-6/qabstractscrollarea.html#setViewportMargins)。此特性主要用于在滚动区域的上方/侧边放置[`QHeaderView`](https://doc.qt.io/qt-6/qheaderview.html)控件。`QAbstractScrollArea`的子类需自行实现 margin。

当继承`QAbstractScrollArea`时，需做以下工作：

- 通过设置滚动条的范围、值、页面步长，并跟踪它们的移动，来控制滚动条
- 根据滚动条的值，在视口中绘制区域的内容
- ==在`viewportEvent()`中处理视口接收的事件==，尤其是 resize 事件
- ==使用`viewport->update()`更新视口的内容，而不是`update()`==，因为所有的绘制操作都发生在视口上。

滚动条的默认策略为[`Qt::ScrollBarAsNeeded`](https://doc.qt.io/qt-6/qt.html#ScrollBarPolicy-enum)：当滚动范围非零时显示滚动条，否则隐藏。

每当视口收到 resize 事件或内容的尺寸改变时，都应更新滚动条和视口。当滚动条的值改变时应当更新视口。滚动条的初始值通常在区域接收新内容时设置。

我们给出一个简单示例，实现了一个可以滚动任意`QWidget`的滚动区域。我们将`QWidget`设为视口的子控件；这样就不需要计算要绘制`QWidget`的哪一部分，只需简单使用`QWidget::move()`移动控件即可。当区域内容或视口尺寸改变时，执行以下操作：

```cpp
QSize areaSize = viewport()->size();
QSize widgetSize = widget->size();

verticalScrollBar()->setPageStep(areaSize.height());
horizontalScrollBar()->setPageStep(areaSize.width());
verticalScrollBar()->setRange(0, widgetSize.height() - areaSize.height());
horizontalScrollBar()->setRange(0, widgetSize.width() - areaSize.width());
updateWidgetPosition();
```

当滚动条的值改变时，我们需要更新控件的位置，也就是找到要在视口中绘制的控件部分：

```cpp
int hvalue = horizontalScrollBar()->value();
int vvalue = verticalScrollBar()->value();
QPoint topLeft = viewport()->rect().topLeft();

widget->move(topLeft.x() - hvalue, topLeft.y() - vvalue);
```

要跟踪滚动条的移动，请重写虚函数[`scrollContentsBy()`](https://doc.qt.io/qt-6/qabstractscrollarea.html#scrollContentsBy)。要对滚动行为做一些微调，请连接滚动条的[`QAbstractSlider::actionTriggered()`](https://doc.qt.io/qt-6/qabstractslider.html#actionTriggered)信号并根据需要调整[`QAbstractSlider::sliderPosition`](https://doc.qt.io/qt-6/qabstractslider.html#sliderPosition-prop)。

为了方便，`QAbstractScrollArea`使所有视口事件在[`viewportEvent()`](https://doc.qt.io/qt-6/qabstractscrollarea.html#viewportEvent)中都可用。在有意义的情况下，`QWidget`的专用事件处理函数会被重映射到视口事件。可以重映射的事件处理函数有：`paintEvent()`，`mousePressEvent()`，`mouseReleaseEvent()`，`mouseDoubleClickEvent()`，`mouseMoveEvent()`，`wheelEvent()`，`dragEnterEvent()`，`dragMoveEvent()`，`dragLeaveEvent()`，`dropEvent()`，`contextMenuEvent()`，`resizeEvent()`。

==[`QScrollArea`](https://doc.qt.io/qt-6/qscrollarea.html)继承自`QAbstractScrollArea`，为任意的`QWidget`提供了逐像素的平滑滚动效果==。只有当你需要更特殊的行为时才应子类化`QAbstractScrollArea`。例如，当区域内容不适合通过`QWidget`呈现（例如地图，直接通过视口的`paintEvent()`呈现），或者不希望平滑滚动时。

# Public Functions

##### `QWidget *viewport() const`

返回视口控件。