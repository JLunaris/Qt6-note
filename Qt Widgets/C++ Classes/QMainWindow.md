https://doc.qt.io/qt-6/qmainwindow.html

`QMainWindow`类提供应用程序的主窗口。

- Inherits：[[QWidget]]

# Public Functions

### 中心控件

##### `void setCentralWidget(QWidget *widget)`

将 *widget* 设为主窗口的中心控件。

`QMainWindow`将接管 *widget* 的所有权。

##### `QWidget *centralWidget() const`

返回主窗口的中心控件。如果未设置中心控件，返回`nullptr`。