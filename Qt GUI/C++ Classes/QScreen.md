https://doc.qt.io/qt-6/qscreen.html

`QScreen`类用于查询屏幕属性。

- Inherits：[[QObject]]

# Properties

### geometry : const QRect

该属性保存屏幕的几何信息，以**像素**为单位。

例如，这可能返回`QRect(0, 0, 1280, 1024)`；或在虚拟桌面环境中，可能返回`QRect(1280, 0, 1280, 1024)`。

| 访问函数： |                  |
| ----: | ---------------- |
| QRect | geometry() const |

| 通知信号： |                                        |
| ----: | -------------------------------------- |
|  void | geometryChanged(const QRect &geometry) |

### size : const QSize

该属性保存屏幕的像素分辨率。

| 访问函数： |              |
| ----: | ------------ |
| QSize | size() const |

| 通知信号： |                                        |
| ----: | -------------------------------------- |
|  void | geometryChanged(const QRect &geometry) |

### devicePixelRatio : const qreal

该属性保存屏幕的**物理像素**和**设备无关像素**（device-independent pixels）的比值。

普通显示屏的比率通常为`1.0`，"retina"显示屏的比率通常为`2.0`。更高的值也是可能的。

> 设备无关像素：即与设备无关的像素。

> 注意：某些平台上，窗口和其所在屏幕的`devicePixelRatio`可能不同。因此，==仅当您不知道目标窗口时使用此函数。如果您知道目标窗口，请使用`QWindow::devicePixelRatio()`。==

| 访问函数： |                          |
| ----: | ------------------------ |
| qreal | devicePixelRatio() const |

| 通知信号： |                                       |
| ----: | ------------------------------------- |
|  void | physicalDotsPerInchChanged(qreal dpi) |

### refreshRate : const qreal

该属性保存屏幕的（大致）**垂直刷新率**，单位为 Hz 。

> 警告：避免使用屏幕的刷新率来驱动动画（例如通过定时器[`QChronoTimer`](https://doc.qt.io/qt-6/qchronotimer.html)）。而是使用`QWindow::requestUpdate()`。

| 访问函数： |                     |
| ----: | ------------------- |
| qreal | refreshRate() const |

| 通知信号： |                                       |
| ----: | ------------------------------------- |
|  void | refreshRateChanged(qreal refreshRate) |
