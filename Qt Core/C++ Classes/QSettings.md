https://doc.qt.io/qt-6/qsettings.html

`QSettings`类提供了持久、跨平台的应用程序设置。

- Inherits：[[QObject]]

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

注意：以下函数是[线程安全的](https://doc.qt.io/qt-6/threads-reentrancy.html)：[`registerFormat()`](https://doc.qt.io/qt-6/qsettings.html#registerFormat)。


# Public Functions

### 构造和析构

##### `QSettings(const QString &fileName, QSettings::Format format, QObject *parent = nullptr)`

构造一个`QSettings`对象，用于**访问**存储在文件 *fileName* 中的设置，父对象为`parent`。如果文件尚未存在，则创建该文件。

如果 *format* 是[`QSettings::NativeFormat`](https://doc.qt.io/qt-6/qsettings.html#Format-enum)，则 *fileName* 的含义取决于平台：
- Unix 上：*fileName* 是 INI 文件的名称
- macOS 和 iOS 上：*fileName* 是`.plist`文件的名称
- Windows 上：*fileName* 是==系统注册表中的路径==

如果 *format* 是[`QSettings::IniFormat`](https://doc.qt.io/qt-6/qsettings.html#Format-enum)，则 *fileName* 是 ==INI 文件==的名称。

> 警告：此函数能很好地读取由 Qt 生成的 INI 或`.plist`文件，但在处理由其他程序生成的此类文件时可能会失败，因为有些语法不通用。具体见[此处](https://doc.qt.io/qt-6/qsettings.html#QSettings-2)。

##### `QSettings( 参数列表 )`

###### 重载 1

```cpp
QSettings(
	QSettings::Format format, // 以何种格式存储设置
	QSettings::Scope scope, // settings的范围（用户层级 or 系统层级）
	const QString &organization, // 组织名
	const QString &application = QString(), // 应用程序名
	QObject *parent = nullptr // 父对象
)
```

构造一个`QSettings`对象，用于**访问**==特定组织 *organization*== 下的==应用程序 *application*== 的设置，父对象为`parent`。

如果 *format* 为[`QSettings::NativeFormat`](https://doc.qt.io/qt-6/qsettings.html#Format-enum)，则使用平台原生 API 来存储设置。如果 *format* 是[`QSettings::IniFormat`](https://doc.qt.io/qt-6/qsettings.html#Format-enum)，则使用 INI 格式文件来存储设置。

如果 *scope* 为[`QSettings::UserScope`](https://doc.qt.io/qt-6/qsettings.html#Scope-enum)，则先查找用户层级设置，若未找到，再回退到系统层级设置。如果 *scope* 为[`QSettings::SystemScope`](https://doc.qt.io/qt-6/qsettings.html#Scope-enum)，则忽略用户层级设置，仅访问系统层级设置。

若 *application* 为空，表示仅访问组织层级的设置位置。

###### 重载 2

```cpp
// 在重载1的基础上，省略了 format 参数
QSettings(
	QSettings::Scope scope, // settings的范围（用户层级 or 系统层级）
	const QString &organization, // 组织名
	const QString &application = QString(), // 应用程序名
	QObject *parent = nullptr // 父对象
)
```

省略的 *format* 参数被设为[`QSettings::NativeFormat`](https://doc.qt.io/qt-6/qsettings.html#Format-enum)（即使在调用该构造函数前调用了静态方法`setDefaultFormat()`，调用此构造函数仍会重新设置 *format*）。

###### 重载 3

```cpp
// 在重载2的基础上，省略了 scope 参数
explicit QSettings(
	const QString &organization,
	const QString &application = QString(),
	QObject *parent = nullptr
)
```

案例：

```cpp
QSettings settings {"Moose Tech", "Facturo-Pro"};
```

省略的 *scope* 参数被设为[`QSettings::UserScope`](https://doc.qt.io/qt-6/qsettings.html#Scope-enum)。