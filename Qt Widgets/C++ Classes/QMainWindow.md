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

### 状态栏

##### `void setStatusBar(QStatusBar *statusbar)`

将 *statusbar* 设为主窗口的状态栏。

将状态栏设为`nullptr`会将其从主窗口中移除。`QMainWindow`将接管 *statusbar* 的所有权。

##### `QStatusBar *statusBar() const`

返回主窗口的状态栏。如果状态栏不存在，则函数创建并返回一个空状态栏。

