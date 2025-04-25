https://doc.qt.io/qt-6/qmouseevent.html

- Inherits：[[QSinglePointEvent]]
# Detailed Description

#### 事件触发条件

鼠标事件在以下情况被触发：
1. 鼠标按钮（如鼠标左/中/右键）在控件内被**按下**或**松开**
2. 鼠标光标在控件内被**移动**

鼠标移动事件仅在**按住鼠标按钮移动**时触发，除非使用`QWidget::setMouseTracking()`开启鼠标追踪。

#### 鼠标抓住机制

当鼠标按钮在**控件内**被按下时，Qt 自动抓住鼠标，控件将持续接收鼠标事件（即使鼠标已经移动到了控件外），直到最后鼠标按钮松开。

#### 事件传播

鼠标事件包含一个特殊的接收标志（accept flag），表示接收者是否想要接收该事件。如果你的控件没有处理该鼠标事件，你应当调用`QEvent::ignore()`。鼠标事件将沿父控件链向上传播，直到一个控件接收了它，或被某个事件过滤器消耗。

**注意**：如果鼠标事件被传播到一个设置了`Qt::WA_NoMousePropagation`属性的控件，则鼠标事件不会进一步沿父控件链向上传播。

#### 部分函数的说明

- `modifiers()`函数（继承自`QInputEvent`）可以获取**键盘修饰键**（keyboard modifier keys）
- `position()`函数返回的光标坐标是相对于**接收该鼠标事件的widget或item**的。如果你希望把控件的移动作为鼠标事件的结果，请使用`globalPosition()`返回的全局坐标，或使用`QWidget::mapToParent()`将`position()`返回的坐标转为相对于父控件的坐标，这样可以避免移动控件时发生抖动。
- `QWidget::setEnabled()`函数可用于启用或禁用控件接收鼠标/键盘事件。
# Public Functions