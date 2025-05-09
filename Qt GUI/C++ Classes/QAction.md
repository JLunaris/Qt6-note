https://doc.qt.io/qt-6/qaction.html

`QAction`是用户命令的抽象，可以被添加到不同的用户界面组件中。

- Inherits：[[QObject]]

# Detailed Description

应用程序中许多常用命令可通过**菜单**、**工具栏按钮**或**快捷键**调用。为确保用户无论使用哪种界面执行命令，行为均一致，Qt 将这些命令统一表示为**动作**（action）。

动作（action）可以被添加到用户界面元素（如菜单、工具栏）中，并**会自动保持 UI 同步**。例如，在文本处理器中，如果用户按下工具栏的“加粗”按钮，对应的“加粗”菜单项将会自动勾选。

#### 属性

一个`QAction`可以包含的属性以及对应的 setter 如下：

| 属性               | 设置器              |
| ---------------- | ---------------- |
| 图标               | `setIcon()`      |
| 描述文本             | `setText()`      |
| 图标文本             | `setIconText()`  |
| 快捷键              | `setShortcut()`  |
| 状态文本             | `setStatusTip()` |
| “What's This?”文本 | `setWhatsThis()` |
| 工具提示             | `setToolTip()`   |

==图标和文本——两个最重要的属性，也可以在构造函数中直接设置==。

另外，可通过`setFont()`为动作设置独立字体，例如把动作作为菜单项显示时会使用设置的字体。

#### 生命周期

我们建议将 action 设为**使用它的窗口**的子对象（这便于内存管理，确保动作生命周期与窗口一致）。大多数情况下，action 是应用程序**主窗口**的子对象。

#### 控件应用程序中的`QAction`

`QAction`被创建后，应将其==添加到相应的菜单和工具栏==中，然后==连接到执行该动作的槽==。

通过`QWidget::addAction()`或`QGraphicsWidget::addAction()`将动作添加到控件。注意动作必须添加到控件中后才能使用；即使使用全局快捷键（即[`Qt::ApplicationShortcut`](https://doc.qt.io/qt-6/qt.html#ShortcutContext-enum)），也必须先将动作添加到控件。

动作可以作为独立对象创建，也可以在构建菜单时创建。`QMenu`类提供了便利函数，来创建作为菜单项的动作。

# Public Slots

##### `void trigger()`

This is a convenience slot that calls `activate(Trigger)`.

# Signals

##### `void triggered(bool checked = false)`

当动作被用户激活时，发出该信号。例如，用户点击菜单项/工具栏按钮，或按下动作的快捷键组合，或手动调用`trigger()`时。注意，调用`setChecked()`或`toggle()`**不会**发出此信号。

If the action is checkable, _checked_ is `true` if the action is checked, or `false` if the action is unchecked.

