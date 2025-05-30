https://doc.qt.io/qt-6/qwheelevent.html

鼠标滚轮事件。

- Inherits：[[QSinglePointEvent]]

# Public Functions

##### `QPoint angleDelta() const`

返回滚轮旋转的相对量，单位为 $1/8$ 度。

- **正值**表示滚轮向前滚动（远离用户），**负值**表示滚轮向后滚动（靠近用户）。
- `angleDelta().y()`表示自上次事件以来**垂直滚轮**旋转的角度，`angleDelta().x()`表示**水平滚轮**（如果鼠标有水平滚轮的话）旋转的角度。如果鼠标没有水平滚轮，则`angleDelta().x()`始终为`0`。某些鼠标允许用户倾斜滚轮进行水平滚动，某些触摸板支持水平滚动手势，它们也会出现在`angleDelta().x()`中。

多数鼠标每步为15度，此时delta值为`120`的倍数（因为$15 \div \frac{1}{8} = 120$）。

然而，某些鼠标的滚轮具有更高的分辨率，发送小于`120`（即小于15度）的delta值。这种情况下，你可以选择：累积多个事件的delta值，直到达到`120`再触发滚动；或者部分滚动控件以响应每次滚轮事件。但要提供更自然的体验，在支持的系统上应优先使用[`pixelDelta()`](https://doc.qt.io/qt-6/qwheelevent.html#pixelDelta)。

案例：

```cpp
void MyWidget::wheelEvent(QWheelEvent *event)
{
    QPoint numPixels = event->pixelDelta();
    QPoint numDegrees = event->angleDelta() / 8;

    if (!numPixels.isNull()) {
        scrollWithPixels(numPixels);
    } else if (!numDegrees.isNull()) {
        QPoint numSteps = numDegrees / 15;
        scrollWithDegrees(numSteps);
    }

    event->accept();
}
```

> 注意：在支持**滚动阶段**的平台（macOS）上，下列情况中delta值可能为空：
> 1. 滚动即将开始，但滚动距离尚未发生变化（[`Qt::ScrollBegin`](https://doc.qt.io/qt-6/qt.html#ScrollPhase-enum)）
> 2. 滚动已经结束，且滚动距离不再变化（[`Qt::ScrollEnd`](https://doc.qt.io/qt-6/qt.html#ScrollPhase-enum)）

