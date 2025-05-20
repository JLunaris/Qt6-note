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

##### `~QPainter() noexcept`

销毁 painter。

### 显式启动和关闭

##### `bool begin(QPaintDevice *device)`

在绘制设备`device`上开始绘制操作。成功返回`true`，失败返回`false`。

调用`begin()`时，painter 的所有设置（`setPen()`、`setBrush()`等）都将**重置为默认值**。

可能出现的错误如下：

```cpp
painter->begin(0); // impossible - paint device cannot be 0

QPixmap image(0, 0);
painter->begin(&image); // impossible - image.isNull() == true;

painter->begin(myWidget);
painter2->begin(myWidget); // impossible - only one painter at a time
```

注意在大部分情况下，可直接使用构造函数`QPainter(QPaintDevice *)`而非显式调用`begin()`，并且`end()`会在析构时自动调用。

> 警告：==一个绘制设备在同一时间只能被一个`QPainter`绘制==。

> 警告：不支持在格式为[`QImage::Format_Indexed8`](https://doc.qt.io/qt-6/qimage.html#Format-enum)的`QImage`上绘制。

##### `bool end()`

结束绘制，释放绘制过程中使用的所有资源。==通常情况下无需手动调用==，因为在`QPainter`析构时会自动调用此函数。

如果 painter 不再处于活动状态，返回`true`；否则返回`false`。

##### `bool isActive() const`

如果调用了`begin()`但还没调用`end()`，返回`true`；否则返回`false`。

### 渲染提示

##### `void setRenderHint(QPainter::RenderHint hint, bool on = true)`

如果`on`为`true`，则在 painter 上设置给定的渲染提示`hint`；否则清除该渲染提示。

相关内容：[QPainter::RenderHint](https://doc.qt.io/qt-6/qpainter.html#RenderHint-enum)。

##### `void setRenderHints(QPainter::RenderHints hints, bool on = true)`

如果`on`为`true`，则在 painter 上设置给定的一组渲染提示`hints`；否则清除这些渲染提示。

##### `QPainter::RenderHints renderHints() const`

返回为该 painter 设置的所有渲染提示。

##### `bool testRenderHint(QPainter::RenderHint hint) const`

如果`hint`被设置，返回`true`；否则返回`false`。