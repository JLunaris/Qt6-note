https://doc.qt.io/qt-6/qdomdocument.html

`QDomDocument`代表一个 XML 文档。

- Inherits：[[QDomNode]]
- CMake：
```cmake
find_package(Qt6 REQUIRED COMPONENTS Xml)
target_link_libraries(mytarget PRIVATE Qt6::Xml)
```

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Public Functions

### 构造和析构

##### `QDomDocument()`

构造一个空文档。

##### `explicit QDomDocument(const QDomDocumentType &doctype)`

创建一个文档，文档类型为 *doctype*。

相关内容：[`QDomDocumentType`](https://doc.qt.io/qt-6/qdomdocumenttype.html)类

##### `explicit QDomDocument(const QString &name)`

创建一个文档，文档类型的名称为 *name*。

##### `QDomDocument(const QDomDocument &document)`

拷贝构造函数。

拷贝的数据是共享的（**浅拷贝**）：修改一个结点会同时更改另一个文档中的对应结点。

如果想要深拷贝，请使用`cloneNode()`。

##### `~QDomDocument() noexcept`

销毁对象并释放其资源。

### 解析XML文档

##### `QDomDocument::ParseResult setContent(参数列表)`

```cpp
QDomDocument::ParseResult setContent(QAnyStringView text, QDomDocument::ParseOptions options = ParseOption::Default)

QDomDocument::ParseResult setContent(QIODevice *device, 
QDomDocument::ParseOptions options = ParseOption::Default)

QDomDocument::ParseResult setContent(QXmlStreamReader *reader, 
QDomDocument::ParseOptions options = ParseOption::Default)

QDomDocument::ParseResult setContent(const QByteArray &data,
QDomDocument::ParseOptions options = ParseOption::Default)
```

该函数从字符串视图 *text*、IO设备 *device*、流读取器 *reader*、字节数组 *data* 中==解析 XML 文档，并将其设为文档的内容==。它会尝试根据 XML 规范来检测文档的编码（encoding）。返回解析结果，类型为[`ParseResult`](https://doc.qt.io/qt-6/qdomdocument-parseresult.html)（可显式转换为`bool`）。

你可以使用 *options* 参数来指定不同的解析选项，例如启用名称空间处理等。

默认**不启用**名称空间处理。如果不启用，解析器在读取 XML 文件时不进行名称空间处理。此时，[`QDomNode::prefix()`](https://doc.qt.io/qt-6/qdomnode.html#prefix)、[`QDomNode::localName()`](https://doc.qt.io/qt-6/qdomnode.html#localName)和[`QDomNode::namespaceURI()`](https://doc.qt.io/qt-6/qdomnode.html#namespaceURI)函数将返回空字符串。

如果通过解析选项 *options* 启用了名称空间处理，解析器将在 XML 文件中识别名称空间，并设置前缀名称、局部名称和名称空间 URI 为合适的值。此时，[`QDomNode::prefix()`](https://doc.qt.io/qt-6/qdomnode.html#prefix)、[`QDomNode::localName()`](https://doc.qt.io/qt-6/qdomnode.html#localName)和[`QDomNode::namespaceURI()`](https://doc.qt.io/qt-6/qdomnode.html#namespaceURI)函数会为任意元素或属性返回字符串；如果元素或属性没有前缀，则返回空字符串。

**只包含空白字符的文本结点会被剔除**，不会出现在`QDomDocument`中。可传递[`QDomDocument::ParseOption::PreserveSpacingOnlyNodes`](https://doc.qt.io/qt-6/qdomdocument.html#ParseOption-enum)给 *options* 来保留仅由空白组成的文本结点。

实体引用（entity reference）的处理方式详见[官方文档](https://doc.qt.io/qt-6/qdomdocument.html#setContent)。

> 注意：对于`[重载2]`（首参数为`QIODevice *`），`setContent()`不会自动打开设备。因此，应用程序应在调用`setContent()`前手动打开设备。

### 元素

##### `QDomElement documentElement() const`

返回文档的**根结点**元素。

### 其他

##### `QString toString(int indent = 1) const`

将解析后的文档转换回**文本表示**。

*indent* 指定元素层级之间的缩进空格数。

如果 *indent* 为`-1`，则不添加任何空格。

