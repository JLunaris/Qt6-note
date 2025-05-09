https://doc.qt.io/qt-6/qaction.html

`QAction`是用户命令的抽象，可以被添加到不同的用户界面组件中。

- Inherits：[[QObject]]

# Detailed Description

应用程序中许多常用命令可通过**菜单**、**工具栏按钮**或**快捷键**调用。为确保用户无论使用哪种界面执行命令，行为均一致，Qt 将这些命令统一表示为**动作**（action）。

动作（action）可以被添加到用户界面元素（如菜单、工具栏）中，并**会自动保持 UI 同步**。例如，在文本处理器中，如果用户按下工具栏的“加粗”按钮，对应的“加粗”菜单项将会自动勾选。

一个`QAction`可以包含 icon、descriptive text、icon text、keyboard shortcut、status text、“What's This?” text、tooltip。所有属性都可通过`setIcon()`、`setText()`、`setIconText()`、`setShortcut()`、`setStatusTip()`、`setWhatsThis()`、`setToolTip()`独立设置。==图标和文本——两个最重要的属性，也可以在构造函数中直接设置==。可通过`setFont()`为动作设置独立字体，例如把动作作为菜单项显示时会使用设置的字体。

我们强烈建议将 action 设为使用它的窗口的子对象（这便于内存管理，确保动作生命周期与窗口一致）。大多数情况下，action 是应用程序**主窗口**的子对象。