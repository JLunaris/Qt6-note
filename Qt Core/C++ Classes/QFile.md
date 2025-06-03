https://doc.qt.io/qt-6/qfile.html

`QFile`类提供**读取、写入**文件的接口。

- Inherits：[[QFileDevice]]
- Inherited By：[QTemporaryFile](https://doc.qt.io/qt-6/qtemporaryfile.html)

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Detailed Description

### Reading Files Directly

下列代码逐行读取文本文件：

```cpp
QFile file("in.txt");
if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
	return;

while (!file.atEnd()) {
	QByteArray line = file.readLine();
	process_line(line);
}
```

传给`open()`的 [Text](https://doc.qt.io/qt-6/qiodevicebase.html#OpenModeFlag-enum) 标志告诉 Qt 将 **Windows 风格**的行结束符(`"\r\n"`)转换为 **C++ 风格**的行结束符(`"\n"`)。默认情况下，`QFile`以二进制模式处理文件，即不会对文件中的字节进行任何转换。

# Public Functions

### 构造和析构

##### `QFile()`

构造一个`QFile`对象。

##### `explicit QFile(QObject *parent)`

构造一个`QFile`对象，父对象为 *parent*。

##### `explicit QFile(const QString &name)`

构造一个新文件对象，表示给定 *name* 的文件。

##### `explicit QFile(const std::filesystem::path &name)`

构造一个新文件对象，表示给定 *name* 的文件。

##### `QFile(const QString &name, QObject *parent)`

构造一个新文件对象，父对象为 *parent*，表示给定 *name* 的文件。

##### `QFile(const std::filesystem::path &name, QObject *parent)`

构造一个新文件对象，父对象为 *parent*，表示给定 *name* 的文件。

##### `virtual ~QFile() noexcept`

销毁文件对象，并在有需要时关闭文件。

### 文件名

##### `void setFileName(const QString &name)`

设置文件的名称 *name*。名称可以是**不带路径**的文件名、**相对路径**或**绝对路径**。如果 *name* 不带路径或是相对路径，则在调用`open()`时，使用的是**应用程序的当前目录路径**。

如果文件已经被打开，不要调用该函数。

案例：

```cpp
QFile file;
QDir::setCurrent("/tmp");
file.setFileName("readme.txt");
QDir::setCurrent("/home");
file.open(QIODevice::ReadOnly);      // opens "/home/readme.txt" under Unix
```

注意，Qt 支持的所有操作系统均可使用`"/"`作为目录分隔符（即使在 Windows 上也不需要写成 `"\\"`）。

# Reimplemented Public Functions

##### `virtual bool open(QIODeviceBase::OpenMode mode) override`

Reimplements：`QIODevice::open(QIODeviceBase::OpenMode mode)`

使用 *mode* 模式打开文件，成功返回`true`，失败返回`false`。

*mode* 标志必须包含以下三者之一：[`ReadOnly`](https://doc.qt.io/qt-6/qiodevicebase.html#OpenModeFlag-enum)、[`WriteOnly`](https://doc.qt.io/qt-6/qiodevicebase.html#OpenModeFlag-enum)或[`ReadWrite`](https://doc.qt.io/qt-6/qiodevicebase.html#OpenModeFlag-enum)。还可以包含其他标志，如[`Text`](https://doc.qt.io/qt-6/qiodevicebase.html#OpenModeFlag-enum)和[`Unbuffered`](https://doc.qt.io/qt-6/qiodevicebase.html#OpenModeFlag-enum)。

> 注意：在[`WriteOnly`](https://doc.qt.io/qt-6/qiodevicebase.html#OpenModeFlag-enum)或[`ReadWrite`](https://doc.qt.io/qt-6/qiodevicebase.html#OpenModeFlag-enum)模式下，如果相关文件不存在，该函数将尝试创建一个新文件，再打开它。
> 
> - 在 POSIX 系统上，创建的文件权限为 0666 masked by the unmask
> - 在 Windows 系统上，权限继承自父目录
> - 在 Android 系统上，它需要对文件名的父目录具有访问权限，否则无法创建该文件

##### `virtual QString fileName() const override`

Reimplements：`QFileDevice::fileName() const`

返回文件的名称（该名称由`setFileName()`、`rename()`或`QFile`构造函数设置）。

