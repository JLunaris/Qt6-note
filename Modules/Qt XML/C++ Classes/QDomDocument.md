https://doc.qt.io/qt-6/qdomdocument.html

`QDomDocument`代表一个 XML 文档。

- Inherits：[[QDomNode]]
- CMake：
```cmake
find_package(Qt6 REQUIRED COMPONENTS Xml)
target_link_libraries(mytarget PRIVATE Qt6::Xml)
```

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Detailed Description

`QDomDocument`类代表==整个 XML 文档==。从概念上讲，它是文档树的**根结点**，并提供对文档数据的主要访问。

由于元素、文本结点、注释、处理指令等都不能脱离文档上下文而独立存在，因此该类还还提供了创建这些对象所需的工厂函数。创建的结点对象都具有[`ownerDocument()`](https://doc.qt.io/qt-6/qdomnode.html#ownerDocument)函数，用于返回结点所属的文档。最常使用的 DOM 类包括：`QDomNode`、`QDomDocument`、`QDomElement`和`QDomText`。

解析后的 XML 会在内部表示为一个对象树，可通过各种`QDom`类进行访问。==所有`QDom`类仅仅**引用**内部树中的对象==。==一旦最后一个引用它们的`QDom`对象被删除，或是`QDomDocument`本身被删除，内部 DOM 树将被销毁==。

> 个人笔记：测试代码如下，离开`f()`作用域后，局部变量`doc`被销毁，内部 DOM 树也被销毁，再通过`element.ownerDocument()`访问 DOM 树属于未定义行为。

```cpp
auto f()
{  
    QDomDocument doc;
    QDomElement element {doc.createElement("g")};
    element.setAttribute("age", "13");
    doc.appendChild(element);
    qDebug() << element.ownerDocument().toString();  // 输出"<g age="13"/>"
    return element;
}

int main()
{
    QDomElement element {f()};
    qDebug() << element.ownerDocument().toString();  // 输出空字符串(未定义行为)
}
```

==元素、文本结点等的创建是通过该类提供的各种工厂函数来完成的。使用`QDom`类的默认构造函数只会得到一个空对象，既不能操作，也不能插入到文档中。==

`QDomDocument`类提供了多个用于创建文档数据的函数，如：`createElement()`、`createTextNode()`、`createComment()`、`createCDATASection()`、`createProcessingInstruction()`、`createAttribute()`和`createEntityReference()`。其中一些函数还提供了支持名称空间的版本，如`createElementNS()`和`createAttributeNS()`。函数`createDocumentFragment()`用于创建文档片段，在处理复杂文档时非常有用。

整个文档的内容可以通过`setContent()`设置。该函数会将传入的字符串作为 XML 文档进行解析，并构建代表该文档的 DOM 树。根元素可通过`documentElement()`获取；文档的文本形式可通过`toString()`获得。

> 注意：如果 XML 文档较大，DOM 树可能会占用大量内存。对于这类文档，使用 [`QXmlStreamReader`](https://doc.qt.io/qt-6/qxmlstreamreader.html)类可能是更好的选择。

[`importNode()`](https://doc.qt.io/qt-6/qdomdocument.html#importNode)用于==将另一个文档中的结点插入到当前文档==。

要获得 DOM 树中具有特定标签的所有元素，使用[`elementsByTagName()`](https://doc.qt.io/qt-6/qdomdocument.html#elementsByTagName)或[`elementsByTagNameNS()`](https://doc.qt.io/qt-6/qdomdocument.html#elementsByTagNameNS)。

`QDom`类的典型用法如下：

```cpp
QDomDocument doc("mydocument");
QFile file("mydocument.xml");
if (!file.open(QIODevice::ReadOnly))
	return;
if (!doc.setContent(&file)) {
	file.close();
	return;
}
file.close();

// print out the element names of all elements that are direct children
// of the outermost element.
QDomElement docElem = doc.documentElement();

QDomNode n = docElem.firstChild();
while(!n.isNull()) {
	QDomElement e = n.toElement(); // try to convert the node to an element.
	if(!e.isNull()) {
		cout << qPrintable(e.tagName()) << '\n'; // the node really is an element.
	}
	n = n.nextSibling();
}

// Here we append a new element to the end of the document
QDomElement elem = doc.createElement("img");
elem.setAttribute("src", "myimage.png");
docElem.appendChild(elem);
```

一旦`doc`和`elem`离开作用域，代表 XML 文档的整个内部树将会销毁。

要使用 DOM 创建一个文档，可以参考如下代码：

```cpp
QDomDocument doc;
QDomElement root = doc.createElement("MyML");
doc.appendChild(root);

QDomElement tag = doc.createElement("Greeting");
root.appendChild(tag);

QDomText t = doc.createTextNode("Hello World");
tag.appendChild(t);

QString xml = doc.toString();
```

关于**文档对象模型**（DOM）的更多信息，请参考 [Level 1](http://www.w3.org/TR/REC-DOM-Level-1/) 和 [Level 2 Core](http://www.w3.org/TR/DOM-Level-2-Core/) 规范。

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

==拷贝的数据是共享的（**浅拷贝**）==：修改一个结点会同时更改另一个文档中的对应结点。

如果想要深拷贝，请使用[`cloneNode()`](https://doc.qt.io/qt-6/qdomnode.html#cloneNode)。

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

相关内容：[QDomDocument::ParseResult](https://doc.qt.io/qt-6/qdomdocument-parseresult.html)类

### 创建结点

```cpp
1. QDomAttr createAttribute(const QString &name)
2. QDomAttr createAttributeNS(const QString &nsURI, const QString &qName)
3. QDomCDATASection createCDATASection(const QString &value)
4. QDomComment createComment(const QString &value)
5. QDomDocumentFragment createDocumentFragment()
6. QDomElement createElement(const QString &tagName)
7. QDomElement createElementNS(const QString &nsURI, const QString &qName)
8. QDomEntityReference createEntityReference(const QString &name)
9. QDomProcessingInstruction createProcessingInstruction(const QString &target, const QString &data)
10. QDomText createTextNode(const QString &value)
```

详见 [QDomDocument](https://doc.qt.io/qt-6/qdomdocument.html#public-functions)。要创建新结点，**必须且只能通过上述方法**，不能使用结点类型的默认构造函数创建新结点。

### 元素

##### `QDomElement documentElement() const`

返回文档的**根元素**。

> XML 文档**有且仅有一个**根元素。

### 文本表示

##### `QString toString(int indent = 1) const`

将解析后的文档转换回**文本表示**。

*indent* 指定元素层级之间的缩进空格数。

如果 *indent* 为`-1`，则不添加任何空格。

