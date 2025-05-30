https://doc.qt.io/qt-6/qstatusbar.html

`QStatusBar`是一个水平状态栏，适合显示状态信息。

- Inherits：[[QWidget]]

# Public Functions

# Detailed Description

状态指示器分以下3类：

- 临时的：临时占据状态栏的大部分空间。例如用于显示工具提示文本或菜单项的说明。
- 普通的：占据状态栏的一部分，可能会被临时信息覆盖。例如在文字处理器中显示当前页码和行号。
- 永久的：永不隐藏。用于重要模式指示，例如，某些应用程序在状态栏中放置“大写锁定”指示。

`QStatusBar`允许你显示这三种类型的指示器。

使用`showMessage()`槽函数来显示**临时**信息：

```cpp
statusBar()->showMessage(tr("Ready"));
```

要**移除临时信息**，使用`clearMessage()`槽函数，或在调用`showMessage()`时设置时间限制。例如：

```cpp
statusBar()->showMessage(tr("Ready"), 2000);
```

使用`currentMessage()`函数获取当前正在显示的临时信息。`QStatusBar`也提供了`messageChanged()`信号，每当临时信息改变时就发出该信号。

**普通**和**永久**信息的显示方式是：创建一个小控件（`QLabel`，`QProgressBar`，甚至`QToolButton`），然后通过`addWidget()`或`addPermanentWidget()`函数将其加入状态栏。要移除这样的信息，使用`removeWidget()`函数。

```cpp
statusBar()->addWidget(new MyReadWriteIndication);
```

默认情况下，`QStatusBar`会在右下角提供一个[`QSizeGrip`](https://doc.qt.io/qt-6/qsizegrip.html)。你可以使用`setSizeGripEnabled()`函数禁用它。使用`isSizeGripEnabled()`函数判断它是否被启用。

![[Pasted image 20250530171606.png]]

