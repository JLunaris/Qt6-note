https://doc.qt.io/qt-6/qgradient.html

`QGradient`类与`QBrush`结合使用，以指定渐变填充。

- Inherited By：[[QLinearGradient]]，[QConicalGradient](https://doc.qt.io/qt-6/qconicalgradient.html)，[QRadialGradient](https://doc.qt.io/qt-6/qradialgradient.html)

# Detailed Description

Qt 支持3种类型的渐变填充：

- **线性渐变（linear gradient）**：在开始点和结束点之间进行颜色插值
- **径向渐变（radial gradient）**
	- 简单径向渐变（simple radial gradient）：在焦点和环绕它的圆上的结束点之间进行颜色插值
	- 扩展径向渐变（extended radial gradient）：在中心和焦点圆之间进行颜色插值
- **锥形渐变（conical gradient）**：围绕中心点进行环形颜色插值


可使用`type()`获取渐变类型，有以下3种（它们都是`QGradient`的子类）：

| [QLinearGradient](https://doc.qt.io/qt-6/qlineargradient.html) | [QRadialGradient](https://doc.qt.io/qt-6/qradialgradient.html) | [QConicalGradient](https://doc.qt.io/qt-6/qconicalgradient.html) |
| -------------------------------------------------------------- | -------------------------------------------------------------- | ---------------------------------------------------------------- |
| ![](https://doc.qt.io/qt-6/images/qgradient-linear.png)        | ![](https://doc.qt.io/qt-6/images/qgradient-radial.png)        | ![](https://doc.qt.io/qt-6/images/qgradient-conical.png)         |

# Public Functions

### 停止点
##### `void setColorAt(qreal position, const QColor &color)`

在 *position* 处创建一个停止点，颜色为 *color*。

*position* 的范围是 ==0~1==。

##### `void setStops(const QGradientStops &stopPoints)`

用给定的 *stopPoints* 替换当前的停止点列表。所有点都必须在 0~1 范围内，且必须**升序排列**（即 *position* 值最小的点排在首位）。

相关内容：[[QGradient#Related Non-Members]]

##### `QGradientStops stops() const`

返回停止点列表。

如果未指定任何停止点，则默认使用从 **0处的黑色** 到 **1处的白色** 的渐变。

### 其他
##### `QGradient::Type type() const`

返回渐变的类型（[QGradient::Type](https://doc.qt.io/qt-6/qgradient.html#Type-enum)）。

# Related Non-Members

### QGradientStop

Typedef for `std::pair<qreal, QColor>`.

### QGradientStops

Typedef for `QList<QGradientStop>`.