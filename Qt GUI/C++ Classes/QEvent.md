https://doc.qt.io/qt-6/qevent.html

`QEvent`类是所有事件类的基类。

- Inherited By：[[QInputEvent]] 等

# Public Functions

##### `void ignore()`

清除事件对象的 accept 标志，等价于`setAccepted(false)`。

调用该函数，表明事件接收器==拒绝处理该事件==。拒绝处理的事件可能会**传播到父控件**。

