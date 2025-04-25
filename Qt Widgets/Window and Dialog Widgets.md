# Window Geometry

`QWidget`类提供了若干处理控件几何属性的函数。这些函数中，有些操作纯客户区域（即除去窗口框架的窗口区域），另一些则包含窗口框架。

- 包含窗口框架的函数：`x()`，`y()`，`frameGeometry()`，`pos()`，`move()`
- 不包含窗口框架的函数：`geometry()`，`width()`，`height()`，`rect()`，`size()`

注意，此区分仅对于**带装饰的顶级控件**有意义。对于所有的子控件，框架的geometry与客户区的geometry是**等价**的。

![[Pasted image 20250408101013.png]]