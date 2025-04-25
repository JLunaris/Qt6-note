https://doc.qt.io/qt-6/qpainter.html

`QPainter`类在控件和其他绘制设备（paint device）上执行低级（low-level）绘制。

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Public Functions

### 渲染提示

##### `void setRenderHint(QPainter::RenderHint hint, bool on = true)`

如果`on`为`true`，则在 painter 上设置给定的渲染提示`hint`；否则清除该渲染提示。

##### `void setRenderHints(QPainter::RenderHints hints, bool on = true)`

如果`on`为`true`，则在 painter 上设置给定的一组渲染提示`hints`；否则清除这些渲染提示。

##### `QPainter::RenderHints renderHints() const`

返回为该 painter 设置的所有渲染提示。

##### `bool testRenderHint(QPainter::RenderHint hint) const`

如果`hint`被设置，返回`true`；否则返回`false`。