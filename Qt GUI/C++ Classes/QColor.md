https://doc.qt.io/qt-6/qcolor.html

`QColor`类提供基于RGB、HSV或CMYK值的颜色。

# Detailed Description

颜色通常通过 ==RGB (red, green, blue)== 来指定，但可也通过 ==HSV (hue, saturation, value)== 或 ==CMYK (cyan, magenta, yellow, black)== 来指定。此外，颜色也可通过**颜色名称**来指定，颜色名称可以是 SVG 1.0 中的任意颜色名称。

| RGB                                               | HSV                                               | CMYK                                               |
| ------------------------------------------------- | ------------------------------------------------- | -------------------------------------------------- |
| ![](https://doc.qt.io/qt-6/images/qcolor-rgb.png) | ![](https://doc.qt.io/qt-6/images/qcolor-hsv.png) | ![](https://doc.qt.io/qt-6/images/qcolor-cmyk.png) |

1. `QColor`构造函数基于 RGB 值创建颜色。

2.  [`toHsv()`](https://doc.qt.io/qt-6/qcolor.html#toHsv)或[`toCmyk()`](https://doc.qt.io/qt-6/qcolor.html#toCmyk)：将颜色转换为基于 HSV值 / CMYK值 的`QColor`并返回该副本。      [`convertTo()`](https://doc.qt.io/qt-6/qcolor.html#convertTo)：将颜色转换为基于指定格式的`QColor`并返回该副本。

3. **静态**成员函数[`fromRgb()`](https://doc.qt.io/qt-6/qcolor.html#fromRgb)、[`fromHsv()`](https://doc.qt.io/qt-6/qcolor.html#fromHsv)、[`fromCmyk()`](https://doc.qt.io/qt-6/qcolor.html#fromCmyk)：基于 RGB值 / HSV值 / CMYK值 创建颜色。
   **静态**成员函数[`fromString()`](https://doc.qt.io/qt-6/qcolor.html#fromString)：根据字符串创建颜色。如 RGB字符串（如`"#112233"`）、ARGB字符串（如`"#ff112233"`）、颜色名称（如`"blue"`）。

4. [`setRgb()`](https://doc.qt.io/qt-6/qcolor.html#setRgb)、[`setHsv()`](https://doc.qt.io/qt-6/qcolor.html#setHsv)、[`setCmyk()`](https://doc.qt.io/qt-6/qcolor.html#setCmyk)：基于指定格式，修改颜色。

5. [`spec()`](https://doc.qt.io/qt-6/qcolor.html#spec)：当前`QColor`是用哪种颜色格式表示的。

6. [`name()`](https://doc.qt.io/qt-6/qcolor.html#name)：返回颜色名称（`QString`型），格式为`"#RRGGBB"`。

7. [`lighter()`](https://doc.qt.io/qt-6/qcolor.html#lighter)、[`darker()`](https://doc.qt.io/qt-6/qcolor.html#darker)：返回当前颜色的一个更亮/更暗的副本。

