https://doc.qt.io/qt-6/qsinglepointevent.html

- Inherits：[[QPointerEvent]]
- Inherited By：[[QMouseEvent]] 等

# Public Funtions

##### `QPointF position() const`

返回此事件的点（point）在**接收此事件的 widget 或 item** 中的位置。

##### `QPointF globalPosition() const`

返回此事件的点（point）在**屏幕或虚拟桌面**中的位置。

> 注意：鼠标指针的全局位置是在**事件发生时**（at the time of the event）被记录下来的。这对异步窗口系统（例如X11）很重要，当你移动控件以响应鼠标事件时，`globalPosition()`和由`QCursor::pos()`返回的当前光标位置可能有很大的不同。
>
> 可以这么理解：
> 1. 鼠标事件发生时（如移动鼠标），系统会“拍一张照片”，记下那一刻鼠标的位置，这个位置就是`globalPosition()`的值。
> 2. 在处理该事件的时候，鼠标可能已经移动了（因为事件处理在事件通过时间系统传播后发生，这需要时间来完成）。当调用`QCursor::pos()`时获取的是鼠标移动后的位置。
> 参考自[StackOverflow](https://stackoverflow.com/questions/62946096/whats-the-differences-between-qcursorpos-and-event-globalpos)。

