https://doc.qt.io/qt-6/qfiledialog.html

`QFileDialog`类提供一个对话框（dialog），允许用户选择文件或目录。

- Inherits：[[QDialog]]

# Static Public Members

##### `static QString getSaveFileName( 参数列表 )`

```cpp
QString getSaveFileName(
	QWidget *parent = nullptr,
	const QString &caption = QString(), // 指定对话框的标题。如果未指定，则使用默认标题。
	const QString &dir = QString(), // 工作目录（可以包含文件名，如果包含，表示保存文件时的默认文件名）
	const QString &filter = QString(), // 只有符合filter要求的文件会被显示
	QString *selectedFilter = nullptr, // 用于保存用户选择的过滤器（即：用户最终所选的过滤器会被传递给selectedFilter）
	QFileDialog::Options options = Options() // 指定如何运行对话框
)
```

获取用户选择的保存文件名（该文件不必已存在），并返回。

它会创建一个模态文件对话框，父控件为 *parent*。如果 *parent* 不是`nullptr`，对话框会居中显示在父控件上方。

```Cpp
QString filename = QFileDialog::getSaveFileName(
	this,
	tr("Save File"),
	"/home/jana/untitled.png",
	tr("Images (*.png *.xpm *.jpg)")
);
```

参数 *dir*、*filter*、*selectedFilter* 可以是空字符串。

多个过滤器用`;;`分隔。例如：

```cpp
"Images (*.png *.xpm *.jpg);;Text files (*.txt);;XML files (*.xml)"
```

*options* 参数用于指定如何运行对话框，详见 [QFileDialog::Option](https://doc.qt.io/qt-6/qfiledialog.html#Option-enum)。

可以通过设置 *selectedFilter* 为期望的值来选择默认过滤器。