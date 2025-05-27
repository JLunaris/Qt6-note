https://doc.qt.io/qt-6/qobject.html

QObject类是所有Qt对象的基类。

# Public Functions

### 信号的阻塞

##### `bool blockSignals(bool block) noexcept`

如果 *block* 为`true`，该对象发出的信号会被阻塞（即：发出信号不会导致任何槽函数被调用）。如果 *block* 为`false`，这样的阻塞将不会发生。

返回值为调用该函数前`signalsBlocked()`的值。

注意，==[`destroyed()`](https://doc.qt.io/qt-6/qobject.html#destroyed)信号不会被阻塞==。

阻塞期间发出的信号不会被缓存（即：阻塞期间发出的信号，在解除阻塞后不会进行补发）。

**See also** [[QSignalBlocker]].

##### `bool signalsBlocked() const noexcept`

如果信号被阻塞，返回`true`；否则返回`false`。

默认情况下信号不会被阻塞。

# Macros

### Q_OBJECT

Q_OBJECT宏用于启用**元对象**（meta-object）功能，例如动态属性、信号和槽。

你可以将Q_OBJECT宏添加到的类定义的任何部分。添加了该宏的类定义可以==声明自己的信号和槽==或==使用Qt元对象系统提供的其他服务==。

> 注意：该宏展开以==`private:`==访问说明符结尾。如果在该宏后**立即**声明成员，这些成员也将是 private 的。

案例：

```Cpp
#include <QObject>

class Counter : public QObject
{
    Q_OBJECT

// Note. The Q_OBJECT macro starts a private section.
// To declare public members, use the 'public:' access modifier.
public:
    Counter() { m_value = 0; }

    int value() const { return m_value; }

public slots:
    void setValue(int value);

signals:
    void valueChanged(int newValue);

private:
    int m_value;
};
```

> 注意：==只有类是`QObject`的子类才可以使用该宏==。对于非`QObject`子类的类，需改用`Q_GADGET`或`Q_GADGET_EXPORT`宏来启用元对象系统对该类中的**枚举**的支持。

更多内容见 [[The Meta-Object System]]，[[Signals & Slots]]，[Qt's Property System](https://doc.qt.io/qt-6/properties.html)。

### 信号和槽——5个相关宏

- `Q_SIGNALS`：作用等价于`signals`关键字。
- `Q_SIGNAL`：将单个函数标记为信号。
- `Q_SLOTS`：作用等价于`slots`关键字。
- `Q_SLOT`：将单个函数标记为槽。
- `Q_EMIT`：作用等价于`emit`关键字。

