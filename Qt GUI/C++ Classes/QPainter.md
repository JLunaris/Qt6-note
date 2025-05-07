https://doc.qt.io/qt-6/qpainter.html

`QPainter`类在控件和其他绘制设备（paint device）上执行低级（low-level）绘制。

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Public Functions

### 构造和析构

##### `QPainter()`

构造 painter。

##### `QPainter(QPaintDevice *device)`

构造一个==可以立即在指定绘制设备`device`上绘制==的 painter。

该构造函数可用于构造**短生命周期的** painter，例如在`QWidget::paintEvent()`中使用，并且应**仅使用一次**。==该构造函数为你调用`begin()`，然后`~QPainter()`自动调用`end()`==。

下面是一个使用`begin()`和`end()`的案例：

```cpp
void MyWidget::paintEvent(QPaintEvent *)
{
	QPainter p;
	p.begin(this);
	p.drawLine(drawingCode); // drawing code
	p.end();
}
```

同样的案例，使用该构造函数：

```cpp
void MyWidget::paintEvent(QPaintEvent *)
{
	QPainter p(this);
	p.drawLine(drawingCode); // drawing code
}
```

由于该构造函数无法在 painter 初始化失败时提供反馈，因此在**外部设备**（如 printer）上绘制时应使用`begin()`和`end()`。

##### `~QPainter()`

销毁 painter。

### 渲染提示

##### `void setRenderHint(QPainter::RenderHint hint, bool on = true)`

如果`on`为`true`，则在 painter 上设置给定的渲染提示`hint`；否则清除该渲染提示。

##### `void setRenderHints(QPainter::RenderHints hints, bool on = true)`

如果`on`为`true`，则在 painter 上设置给定的一组渲染提示`hints`；否则清除这些渲染提示。

##### `QPainter::RenderHints renderHints() const`

返回为该 painter 设置的所有渲染提示。

##### `bool testRenderHint(QPainter::RenderHint hint) const`

如果`hint`被设置，返回`true`；否则返回`false`。