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

8. [`isValid()`](https://doc.qt.io/qt-6/qcolor.html#isValid)：返回`QColor`是否完全合法。例如一个 RGB 颜色，如果某个值超出范围就是不合法的。处于性能考虑，`QColor`不会主动检查颜色是否合法，使用无效颜色的结果是未定义的。

9. 颜色的各个分量可以单独获取，例如使用[`red()`](https://doc.qt.io/qt-6/qcolor.html#red)、[`hue()`](https://doc.qt.io/qt-6/qcolor.html#hue)、[`cyan()`](https://doc.qt.io/qt-6/qcolor.html#cyan)。也可以使用[`getRgb()`](https://doc.qt.io/qt-6/qcolor.html#getRgb)、[`getHsv()`](https://doc.qt.io/qt-6/qcolor.html#getHsv)、[`getCmyk()`](https://doc.qt.io/qt-6/qcolor.html#getCmyk)一次性获取所有分量。如果使用的是 RGB 颜色模型，还可通过[`rgb()`](https://doc.qt.io/qt-6/qcolor.html#rgb)获取所有分量值。

10. 相关的非成员：`QRgb`是`unsigned int`的类型别名，表示 RGB 值三元组`(r, g, b)`。此外，它还可以包含 alpha通道的值（更多信息见[Alpha-Blended Drawing](https://doc.qt.io/qt-6/qcolor.html#alpha-blended-drawing)）。

11. `QColor`是平台无关、设备无关的。[`QColormap`](https://doc.qt.io/qt-6/qcolormap.html)类将颜色映射到硬件上。

### 整数与浮点数精度

`QColor`支持浮点数精度，所有的颜色分量函数都有浮点数版本，如`getRgbF()`、`hueF()`、`fromCmykF()`。注意，由于颜色分量使用 **16-bit整数** 存储，通过例如`setRgbF()`设置的值与`getRgbF()`返回的值，两者之间可能存在因舍入而导致的**微小偏差**。

基于**整数**的函数，数值范围为 ==0~255==（除了`hue()`的数值范围为 ==0~359==）；基于**浮点数**的函数，数值范围为 ==0.0~1.0==。

### The HSV Color Model

RGB 模型是面向硬件的，其表示方式与多数显示器的显示原理对应。相比之下，HSV 以更符合人类色彩感知的方式表示颜色，如“更鲜艳”、“更暗”、“互补色”等关系在 HSV 中很容易表达，但在 RGB 中则难以表述。 

HSV 具有3个分量：

- H：色相（hue）。对于彩色（即非灰色）其取值范围为 ==0~359==；对于灰色无意义。它表示色轮上的度数，如图所示。  
![[Pasted image 20250517155722.png]]
- S：饱和度（saturation），范围为 ==0~255==，数值越大色彩越鲜艳。  
  ![[Pasted image 20250517160700.png]]
- V：值（value），范围为 ==0~255==，表示颜色的 **亮度（lightness）** 或 **明度（brightness）**。
  ![[Pasted image 20250517162538.png]]

==对于非彩色，Qt 返回的色相值为 -1==。==如果传递的色相值过大，Qt 会强制它进入范围==，例如 360 或 720 被视为 0，540 被视为 180。

除了标准 HSV 模型外，Qt 还提供了 alpha 通道以支持 [alpha-blended drawing](https://doc.qt.io/qt-6/qcolor.html#alpha-blended-drawing)。

# Public Functions

# Static Public Members

### 构造`QColor`

##### `static QColor fromHsv(int h, int s, int v, int a = 255)`

返回由 HSV 颜色值构造的`QColor`。

_h_ (hue), _s_ (saturation), _v_ (value), and _a_ (alpha-channel, i.e. transparency).

*s*、*v*、*a* 都必须在 ==0~255== 范围内，*h* 必须在 ==0~359== 范围内。

##### `static QColor fromHsvF(float h, float s, float v, float a = 1.0)`

重载版本。所有值都必须在 ==0.0~1.0== 范围内。

##### `static QColor fromString(QAnyStringView name) noexcept`

返回从 *name* 解析得到的 RGB `QColor`，*name* 为以下格式之一：

- `#RGB` (each of R, G, and B is a single hex digit)
- `#RRGGBB`
- `#AARRGGBB` (Since 5.2)
- `#RRRGGGBBB`
- `#RRRRGGGGBBBB`
- 由 W3C 提供的 **SVG 颜色关键字名称**列表中的名称，如`"steelblue"`或`"gainsboro"`。这些颜色名称适用于所有平台。请注意，这些颜色名称与`Qt::GlobalColor`枚举中定义的颜色不同，如`"green"`和`Qt::green`不表示同一个颜色。
- `transparent` - representing the absence of a color.

如果 *name* 无法被解析，返回无效的颜色（可通过`isValid()`判断颜色是否有效）。

### 其他

##### `static bool isValidColorName(QAnyStringView name) noexcept`

如果 *name* 是有效颜色的名称，且可用于构造有效的`QColor`对象，返回`true`。否则返回`false`。

它使用的算法与静态方法`fromString()`一样。

