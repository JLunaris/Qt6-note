https://doc.qt.io/qt-6/qfiledialog.html

`QFileDialog`类提供一个对话框（dialog），允许用户选择文件或目录。

- Inherits：[[QDialog]]

# Static Public Members

##### `static QString getSaveFileName( 参数列表 )`

```cpp
QString getSaveFileName(
	QWidget *parent = nullptr, // 父控件
	const QString &caption = QString(), // 对话框的标题（如果未指定，则使用默认标题）
	const QString &dir = QString(), // 工作目录（可以包含文件名，如果包含，表示要保存的文件的默认文件名）
	const QString &filter = QString(), // 只有符合filter要求的文件会被显示
	QString *selectedFilter = nullptr, // 用于保存用户选择的过滤器（即用户最终选择的过滤器会被传递给selectedFilter）
	QFileDialog::Options options = Options() // 指定如何运行对话框
)
```

==获取用户选择的保存文件名==（该文件不必已存在）并返回。它会创建一个模态（modal）文件对话框，父控件为 *parent*。如果 *parent* 不是`nullptr`，对话框会居中显示在父控件上方。

> 模态（modal）窗口：是一种弹出式的子窗口，它会屏蔽掉原窗口的交互功能，强制用户先处理模态窗口中的事件，才能返回到原窗口。

###### 参数说明

1. 参数 *dir*、*filter*、*selectedFilter* 可以是空字符串。
2. 多个过滤器用`;;`分隔。例如：
```cpp
"Images (*.png *.xpm *.jpg);;Text files (*.txt);;XML files (*.xml)"
```
3. *options* 参数用于指定如何运行对话框，详见 [QFileDialog::Option](https://doc.qt.io/qt-6/qfiledialog.html#Option-enum)。
4. 可以通过设置 *selectedFilter* 为期望的值来选择默认过滤器。

###### 平台特定行为

- 在 Windows 和 macOS 上，该函数使用系统文件对话框，而不是 `QFileDialog`。
- 在 Windows 上，文件对话框会启动一个**阻塞的**模态事件循环，该循环不会派遣任何`QTimer`；并且如果 *parent* 不是`nullptr`，则将对话框定位在父窗口标题栏下方。
- 在 macOS 上，*filter* 实参会被忽略。
- 在 Unix/X11 上，文件对话框的正常行为是解析并跟随符号链接。例如，如果`/usr/tmp`是指向`/var/tmp`的符号链接，则在进入`/usr/tmp`后，会自动切换到`/var/tmp`。如果 *options* 包含 [DontResolveSymlinks](https://doc.qt.io/qt-6/qfiledialog.html#Option-enum)，则将符号链接视为普通目录，不会解析它们。

###### 案例

```Cpp
QString filename = QFileDialog::getSaveFileName(
	this,
	tr("Save File"),
	"/home/jana/untitled.png",
	tr("Images (*.png *.xpm *.jpg)")
);
```

