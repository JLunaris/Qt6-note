https://doc.qt.io/qt-6/qwidget.html

# Properties

### geometry : QRect

1. 该属性保存控件**相对于其父控件**的geometry。如果控件是窗口，不包括其框架。

> 有关窗口的geometry话题概述，详见[[Window and Dialog Widgets#Window Geometry]]

2. 当改变geometry时，若控件处于可见状态，会立即接收一个**移动事件**（`moveEvent()`）和/或一个**resize事件**（`resizeEvent()`）；若控件当前不可见，Qt 会保证它在显示前接收合适的事件。

> 警告：在`resizeEvent()`或`moveEvent()`中调用`setGeometry()`会导致无限递归。

3. 如果尺寸在`minimumSize()`和`maximumSize()`定义的范围之外，则会被自动调整。

|         访问函数： |                                                 |
| ------------: | ----------------------------------------------- |
| const QRect & | geometry() const                                |
|          void | setGeometry(int _x_, int _y_, int _w_, int _h_) |
|          void | setGeometry(const QRect &)                      |
### pos : QPoint

1. 该属性保存控件**相对于其父控件**的位置。如果控件是窗口，则其位置是控件在桌面上的位置，包括其框架。

> 有关窗口的geometry话题概述，详见[[Window and Dialog Widgets#Window Geometry]]

> Note：并非所有窗口系统都支持get/set顶级窗口的位置。在这样的系统中，以编程方式移动窗口可能没有任何效果，返回的当前位置可能是人为值（如`QPoint(0,0)`）。

2. 当改变控件位置时，若它处于可见状态，会立即接收一个**移动事件**（`moveEvent()`）；若控件当前不可见，Qt 会保证它在显示前接收事件。

> 警告：在`moveEvent()`中调用`move()`或`setGeometry()`会导致无限递归。

3. 该属性的默认值为坐标系原点（`QPoint(0,0)`）。

|  访问函数： |                        |
| -----: | ---------------------- |
| QPoint | pos() const            |
|   void | move(int _x_, int _y_) |
|   void | move(const QPoint &)   |
### size : QSize

1. 该属性保存控件的尺寸。如果控件是窗口，不包括其框架。

> 注意：将尺寸设为`QSize(0, 0)`将导致控件不出现在屏幕上。这也适用于窗口。

2. 当改变控件尺寸时，若它处于可见状态，会立即接收一个 **resize 事件**（`resizeEvent()`）；若控件当前不可见，Qt 会保证它在显示前接收事件。

> 警告：在`resizeEvent()`中调用`resize()`或`setGeometry()`会导致无限递归。

3. 如果控件尺寸在`minimumSize()`和`maximumSize()`定义的范围之外，则会被自动调整。

4. 该属性的默认值：取决于用户平台和屏幕geometry。

| 访问函数： |                          |
| ----: | ------------------------ |
| QSize | size() const             |
|  void | resize(int _w_, int _h_) |
|  void | resize(const QSize &)    |

### autoFillBackground : bool

1. 该属性保存是否自动填充控件**背景**。

2. 如果启用，该属性将造成 Qt 在**调用绘制事件（`paintEvent()`）前**填充控件背景。使用的颜色由控件调色板的[`QPalette::Window`](https://doc.qt.io/qt-6/qpalette.html#ColorRole-enum)颜色角色定义。

   此外，如果控件是**窗口**（window），则总是会使用`QPalette::Window`填充它，除非设置了`WA_OpaquePaintEvent`或`WA_NoSystemBackground`属性(attribute)。

3. 如果控件的父控件为静态渐变背景，则该属性无法被关闭（即无法设为`false`）。

4. ==该属性的默认值为`false`==。

> 警告：与 Qt 样式表一起使用时，请谨慎使用此属性。当控件的样式表中设定了有效的 background 或 border-image 时，该属性会被自动禁用。

| 访问函数： |                                     |
| ----: | ----------------------------------- |
|  bool | autoFillBackground() const          |
|  void | setAutoFillBackground(bool enabled) |

# Public Functions

### 构造和析构

##### `explicit QWidget(QWidget *parent = nullptr, Qt::WindowFlags f = Qt::WindowFlags())`

构造一个控件，它是`parent`的子级，控件标志设为`f`。

==如果`parent`为`nullptr`，则新控件将成为**窗口**（window）==。如果`parent`是另一个控件，则该控件将成为`parent`的**子窗口**（child window）。**当`parent`被删除时，该控件也会被删除。**

控件标志`f`默认为`0`，可以显式设置它来定制**窗口的框架**（仅在控件是窗口时才有意义，即`parent`必须是`nullptr`）。可以同时指定多个框架，由任意个窗口标志（[window flags](https://doc.qt.io/qt-6/qt.html#WindowType-enum)）按位或运算（`|`）得到。

> 注意：Qt的X11版本可能无法支持窗口标志的所有组合（这是因为在X11上，Qt只能访问窗口管理器，而窗口管理器可以覆盖应用程序的设置）；在Windows上，Qt可以设置任何你想要的标志。

 如果你向一个**已经可见的控件**中添加子控件，则必须显式`show()`该子控件才能使其可见。

##### `virtual ~QWidget() noexcept`

销毁该控件。

该控件的**所有子控件会被先删除**。如果该控件是主控件，则应用程序退出。

### 位置和大小

##### `void setGeometry(int x, int y, int w, int h)`

等价于`setGeometry(QRect(x, y, w, h))`。

见[[QWidget#Properties#geometry QRect]]

##### `void move(int x, int y)`

等价于`move(QPoint(x, y))`。

见[[QWidget#Properties#pos QPoint]]

##### `void resize(int w, int h)`

等价于`resize(QSize(w, h))`。

见[[QWidget#Properties#size QSize]]

### 坐标转换

##### `QPointF mapFrom(const QWidget *parent, const QPointF &pos) const`

将 ***parent* 的坐标系统坐标**转换为控件坐标。*parent* 不能是`nullptr`，且必须是调用控件的父控件。
##### `QPoint mapFrom(const QWidget *parent, const QPoint &pos) const`

这是一个重载函数。
##### `QPointF mapFromGlobal(const QPointF &pos) const`

将**全局屏幕坐标** *pos* 转换为控件坐标。
##### `QPoint mapFromGlobal(const QPoint &pos) const`

这是一个重载函数。
##### `QPointF mapFromParent(const QPointF &pos) const`

将**父控件坐标** *pos* 转换为控件坐标。

如果控件没有父级，则等同于`mapFromGlobal()`。
##### `QPoint mapFromParent(const QPoint &pos) const`

这是一个重载函数。
##### `QPointF mapTo(const QWidget *parent, const QPointF &pos) const`

将控件坐标 *pos* 转换为 ***parent* 的坐标系统坐标**。*parent* 不能是`nullptr`，且必须是调用控件的父控件。

##### `QPointF mapToGlobal(const QPointF &pos) const`

将控件坐标 *pos* 转换为**全局屏幕坐标**。例如，`mapToGlobal(QPointF(0,0))`将给出控件左上角像素的全局坐标。
##### `QPoint mapToGlobal(const QPoint &pos) const`

这是一个重载函数。
##### `QPointF mapToParent(const QPointF &pos) const`

将控件坐标 *pos* 转换为**父控件坐标**。

如果控件没有父级，则等同于`mapToGlobal()`。
##### `QPoint mapToParent(const QPoint &pos) const`

这是一个重载函数。

### 动作

##### `void addAction(QAction *action)`

向控件的**动作列表**追加动作 *action*。

> 动作列表：每个`QWidget`都有一个`QAction`列表。这些`QAction`有多种图形化的呈现方式（如菜单项、工具栏按钮等），具体取决于控件的实现。通过`actions()`可以获取该列表，该列表默认用于创建上下文菜单（`QMenu`）。

> 上下文菜单：界面中右击鼠标会出现的俗称的“右键菜单”。因为它根据鼠标位置来判断弹出什么菜单，所以称为上下文菜单。

`QWidget`中同一动作只能存在一份；尝试添加一个已存在的动作不会导致其被再次添加。

==*action* 的所有权不会被转移给该`QWidget`==。

##### `void insertAction(QAction *before, QAction *action)`

向控件的动作列表插入动作 *action*，在动作 *before* 前。如果 *before* 为`nullptr`或 *before* 不是该控件的有效动作，则将 *action* 追加到末尾。

`QWidget`中每个动作只能有一个。

##### `void removeAction(QAction *action)`

从控件的动作列表移除动作 *action*。

##### `QList<QAction *> actions() const`

返回控件的动作列表（可能为空）。

# Protected Functions

##### `virtual void resizeEvent(QResizeEvent *event)`

1. 用于接收`resize`事件（当控件尺寸被修改时，自动触发`resizeEvent`）。
2. 当`resizeEvent()`被调用时，控件已经具有新尺寸，可通过`QResizeEvent::oldSize()`获取旧尺寸。
3. `resizeEvent()`调用结束后，控件会被擦除并自动触发`paintEvent()`进行重绘，因此在`resizeEvent()`中**不需要也不应该进行绘制**。

##### `virtual void moveEvent(QMoveEvent *event)`

1. 用于接收控件移动事件。
2. 当`moveEvent()`被调用时，控件已经具有新位置，可通过`QMoveEvent::oldPos()`获取旧位置。

##### `virtual void mousePressEvent(QMouseEvent *event)`

1. 用于接收鼠标按下事件。
2. 默认实现行为：
	- 对弹出式控件(popup widgets)，点击窗口外部时会关闭该控件
	- 对其他类型控件，不执行任何操作。
3. 若在`mousePressEvent()`内创建新控件，可能导致`mouseReleaseEvent()`的触发位置不符合预期，取决于底层窗口系统、控件位置等因素。

##### `virtual void mouseReleaseEvent(QMouseEvent *event)`

1. 用于接收鼠标释放事件。

##### `virtual void mouseMoveEvent(QMouseEvent *event)`

1. 用于接收鼠标移动事件。
2. 鼠标追踪（mouse tracking）行为说明：
	- 鼠标追踪关闭：仅在按住鼠标按钮移动时，才会触发鼠标移动事件
	- 鼠标追踪开启：即使没有按下鼠标按钮，仍会发生鼠标移动事件
3. `QMouseEvent::pos()`返回鼠标光标**相对于当前控件**的位置。对于按下和释放事件，位置通常与**最后一次鼠标移动事件**的位置相同，但可能因用户手部抖动而产生差异（此特性源于底层窗口系统的实现机制，与Qt无关）
4. 如果你想在鼠标移动时立即显示提示框（tooltip）（例如使用`QMouseEvent::pos()`获取鼠标坐标并将其显示为tooltip），你必须首先开启鼠标追踪。然后，为了确保tooltip被实时更新，在你实现的`mouseMoveEvent()`中，你必须调用`QTooltip::showText()`而非`setToolTip()`。