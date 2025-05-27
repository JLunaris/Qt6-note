https://doc.qt.io/qt-6/qsignalblocker.html

对[`QObject::blockSignals()`](https://doc.qt.io/qt-6/qobject.html#blockSignals)的**异常安全封装类**。

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Detailed Description

`QSignalBlocker`可用于替代你原本通过一对[`QObject::blockSignals()`](https://doc.qt.io/qt-6/qobject.html#blockSignals)调用实现的信号阻塞逻辑。它在构造函数中阻塞信号，在析构函数中将信号阻塞状态恢复为构造函数执行前的状态。

因此，除了使用`QSignalBlocker`的代码**在面对异常时是安全的**外，

```cpp
{
const QSignalBlocker blocker{someQObject};
// no signals here
}
```

等价于

```cpp
const bool wasBlocked = someQObject->blockSignals(true);
// no signals here
someQObject->blockSignals(wasBlocked);
```

# Public Functions

### 构造和析构

##### `explicit QSignalBlocker(QObject *object) noexcept`

构造函数。调用`object->blockSignals(true)`。

##### `~QSignalBlocker() noexcept`

析构函数。==将[`QObject::signalsBlocked()`](https://doc.qt.io/qt-6/qobject.html#signalsBlocked)恢复为构造函数执行前的状态==；除非调用了`unblock()`且之后没有调用`reblock()`，在此情况下析构函数什么也不做。

### 其他

##### `void unblock() noexcept`

**临时**将[`QObject::signalsBlocked()`](https://doc.qt.io/qt-6/qobject.html#signalsBlocked)恢复为构造函数执行前的状态。使用`reblock()`撤销该操作。

`reblock()`和`unblock()`的调用次数不会被计数，因此一次`unblock()`就可以撤销任意次数的`reblock()`调用。

##### `void reblock() noexcept`

重新阻塞信号。

`reblock()`和`unblock()`的调用次数不会被计数，因此一次`reblock()`就可以撤销任意次数的`unblock()`调用。

