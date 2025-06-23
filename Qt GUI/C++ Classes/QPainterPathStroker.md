https://doc.qt.io/qt-6/qpainterpathstroker.html

`QPainterPathStroker`类用于为给定的 painter path 生成可填充的轮廓。

# Detailed Description

通过调用`createStroke()`函数，将给定的`QPainterPath`作为实参传递，可以创建一个表示给定路径的轮廓的新 painter path。然后，可以填充新创建的 painter path ，以绘制原始 painter path 的轮廓。

你可以使用以下函数控制轮廓的各种样式细节（如宽度、端点样式、连接样式和虚线模式）：

- [`setWidth()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#setWidth)
- [`setCapStyle()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#setCapStyle)
- [`setJoinStyle()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#setJoinStyle)
- [`setDashPattern()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#setDashPattern)

其中`setDashPattern()`函数可以接受`Qt::PenStyle`对象，也可以接受一个表示模式的列表。

此外，可以使用[`setCurveThreshold()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#setCurveThreshold)函数指定曲线的阈值，用于控制曲线绘制的粒度（granularity）。默认的阈值是一个经过良好调校的值（0.25），通常不需要修改。但如果你希望曲线看起来更平滑，可以将该值调小。

你还可以使用[`setMiterLimit()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#setMiterLimit)函数来控制生成轮廓时的斜接限制（miter limit）。斜接限制描述了在每个连接处，斜接连接（miter join）最多可以延伸多远。该限制是以线宽为单位指定的，因此像素级的斜接限制为`miterlimit × width`。 这个值仅在连接样式为[`Qt::MiterJoin`](https://doc.qt.io/qt-6/qt.html#PenJoinStyle-enum)时使用。

通过`createStroke()`函数生成的 painter path 应仅用于表示给定 painter path 的轮廓。否则可能会导致未定义的行为。生成的轮廓也需要使用[`Qt::WindingFill`](https://doc.qt.io/qt-6/qt.html#FillRule-enum)规则（该规则为默认设置）。

# Public Functions

##### `QPainterPath createStroke(const QPainterPath &path) const`

生成一个新的路径，该路径是一个可填充的区域，表示==给定 *path* 的轮廓==。

轮廓的各项设计细节都基于当前 stroker 的属性：[`width()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#width)、[`capStyle()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#capStyle)、[`joinStyle()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#joinStyle)、[`dashPattern()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#dashPattern)、[`curveThreshold()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#curveThreshold)和[`miterLimit()`](https://doc.qt.io/qt-6/qpainterpathstroker.html#miterLimit)。

生成的 painter path 应仅用于表示给定 painter path 的轮廓。否则可能会导致未定义的行为。生成的轮廓也需要使用[`Qt::WindingFill`](https://doc.qt.io/qt-6/qt.html#FillRule-enum)规则（该规则为默认设置）。