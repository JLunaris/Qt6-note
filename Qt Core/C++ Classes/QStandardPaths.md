https://doc.qt.io/qt-6/qstandardpaths.html

`QStandardPaths`类提供了一些访问标准路径的成员函数。

# Public Types

### `enum StandardLocation`

详见 [QStandardPaths::StandardLocation](https://doc.qt.io/qt-6/qstandardpaths.html#StandardLocation-enum)。

# Static Public Members

##### `static QString writableLocation(QStandardPaths::StandardLocation type)`

返回由 *type* 指定的要写入的==目录路径==。若无法确定路径，则返回空字符串。

> 注意：返回的目录路径可能不存在；也就是说，可能需要由系统或用户手动创建。

相关内容：[QStandardPaths::StandardLocation](https://doc.qt.io/qt-6/qstandardpaths.html#StandardLocation-enum)

