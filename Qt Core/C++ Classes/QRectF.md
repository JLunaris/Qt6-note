https://doc.qt.io/qt-6/qrectf.html

# Detailed Description


# Public Functions

##### `constexpr void adjust(qreal dx1, qreal dy1, qreal dx2, qreal dy2) noexcept`

将`dx1`、`dy1`、`dx2`、`dy2`分别**加**到矩形的现有坐标。

> 【原矩形】左上`(x1, y1)`，右下`(x2, y2)`
> 【新矩形】左上`(x1 + dx1, y1 + dy1)`，右下`(x2 + dx2, y2 + dy2)`

##### `constexpr QRectF adjusted(qreal dx1, qreal dy1, qreal dx2, qreal dy2) const noexcept`

将`dx1`、`dy1`、`dx2`、`dy2`分别**加**到矩形的现有坐标得到一个新矩形，返回该新矩形。

