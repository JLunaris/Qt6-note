https://doc.qt.io/qt-6/metaobjects.html

Qt的**元对象系统**（meta-object system）为对象间通信、运行时类型信息和动态属性系统提供了**信号和槽机制**（signals and slots mechanism）。

元对象系统基于三个方面：

1. `QObject`类：提供了一个基类，使对象能够使用元对象系统的功能。
2. `Q_OBJECT`宏：用于启用元对象特性，如**动态属性**、**信号**、**槽**。
3. 元对象编译器（[Meta-Object Compiler](https://doc.qt.io/qt-6/moc.html)，`moc`）：为每个`QObject`子类生成实现元对象特性所需的代码。

`moc`工具读取一个C++源文件。如果它发现一个或多个包含`Q_OBJECT`宏的类声明，则它会生成另一个C++源文件，其中包含每个这样的类的元对象代码。此生成的源文件要么被`#include`到类的源文件中，要么（更常见的是）被编译并与类的实现链接。

除了为<对象间通信>提供信号和槽机制（引入该系统的主要原因）外，元对象代码还提供了以下额外功能：

- `QObject::metaObject()`：返回该类的元对象。
- `QMetaObject::className()`：在运行时返回类名，而无需依赖 C++ 编译器原生的**运行时类型信息**（run-time type information, RTTI）支持。
- `QObject::inherits()`：判断对象的类型是否在`QObject`继承树中继承了指定的类。
- `QObject::tr()`：翻译字符串，实现国际化。
- `QObject::setProperty()`和`QObject::property()`：根据属性名动态地 get/set 属性。
- `QMetaObject::newInstance()`：创建该类的新实例。

---

虽然可以在没有`Q_OBJECT`宏和元对象代码的情况下将`QObject`作为基类，但如果不使用`Q_OBJECT`宏，信号与槽以及前面提到的其他元对象功能都将无法使用。从元对象系统的角度看，一个没有元对象代码的`QObject`子类等价于其最近的、带有元对象代码的祖先类，这意味着，例如，`QMetaObject::className()`将不会返回你的类的真实名称，而是这个祖先类的名称。

因此，我们强烈建议==所有的`QObject`子类都使用`Q_OBJECT`宏，无论它们是否真的使用了信号、槽和属性==。