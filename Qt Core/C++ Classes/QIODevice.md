https://doc.qt.io/qt-6/qiodevice.html

`QIODevice`类是 Qt 中所有 I/O 设备的基接口类。

- Inherits：[[QObject]] 和 [QIODeviceBase](https://doc.qt.io/qt-6/qiodevicebase.html)
- Inherited By：[[QFileDevice]]

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Public Functions

### 打开和关闭

##### `virtual bool open(QIODeviceBase::OpenMode mode)`

打开设备，并将其 OpenMode 设为 *mode*。成功返回`true`，失败返回`false`。

任何对**该函数**或**其他打开设备的函数**的 reimplements 都应调用该函数。

##### `virtual void close()`

首先发出信号[`aboutToClose()`](https://doc.qt.io/qt-6/qiodevice.html#aboutToClose)，然后关闭设备，并将 OpenMode 设为`NotOpen`。错误字符串也会被重置。

##### `QIODeviceBase::OpenMode openMode() const`

返回设备当前的打开模式。

### 当前位置

##### `virtual bool seek(qint64 pos)`

对于**随机访问（random-access）设备**，该函数==将当前位置设为 *pos*==，成功返回`true`，发生错误时返回`false`。对于**顺序（sequential）设备**，默认行为是产生警告并返回`false`。

在继承`QIODevice`类时，必须在函数起始处调用`QIODevice::seek()`，以便更新`QIODevice`内部的缓冲区状态，确保一致性。

##### `virtual qint64 pos() const`

对于**随机访问设备**，该函数返回当前读/写数据时的位置。对于**顺序设备**或**已关闭的设备**，由于不存在“当前位置”的概念，返回值为`0`。

设备的当前读/写位置由`QIODevice`在内部维护，因此没必要重写此函数。在继承`QIODevice`类时，必须在函数起始处调用`QIODevice::seek()`，以便更新`QIODevice`内部的缓冲区状态，确保一致性。
