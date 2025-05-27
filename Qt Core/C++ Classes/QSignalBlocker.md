https://doc.qt.io/qt-6/qsignalblocker.html

对[`QObject::blockSignals()`](https://doc.qt.io/qt-6/qobject.html#blockSignals)的**异常安全封装类**。

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Public Functions

### 构造和析构

##### `explicit QSignalBlocker(QObject *object) noexcept`

构造函数。调用`object->blockSignals(true)`。

##### `~QSignalBlocker() noexcept`

析构函数。将[`QObject::signalsBlocked()`](https://doc.qt.io/qt-6/qobject.html#signalsBlocked)恢复为构造函数执行前的状态。但有一个例外：如果调用了[`unblock()`](https://doc.qt.io/qt-6/qsignalblocker.html#unblock)，且之后没有调用[`reblock()`]()，在此情况下析构函数什么也不做。

# Detailed Description

