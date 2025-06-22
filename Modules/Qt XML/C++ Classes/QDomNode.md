https://doc.qt.io/qt-6/qdomnode.html

`QDomNode`类是 DOM 树中所有结点的基类。

- Inherited By：[[QDomDocument]]、[[QDomElement]]
- CMake：
```cmake
find_package(Qt6 REQUIRED COMPONENTS Xml)
target_link_libraries(mytarget PRIVATE Qt6::Xml)
```

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Public Functions

### 构造和析构

##### `QDomNode()`

构造一个[空结点](https://doc.qt.io/qt-6/qdomnode.html#isNull)。

##### `QDomNode(const QDomNode &node)`

拷贝构造函数。

==拷贝的数据是共享的（**浅拷贝**）==：修改一个结点会同时更改另一个文档中的对应结点。

如果想要深拷贝，请使用`cloneNode()`。

##### `~QDomNode() noexcept`

销毁对象并释放其资源。

### 结点类型

##### `QDomNode::NodeType nodeType() const`

返回结点的类型。

相关内容：[`QDomNode::NodeType`](https://doc.qt.io/qt-6/qdomnode.html#NodeType-enum)

##### 转换为某结点类型

```cpp
1. QDomAttr toAttr() const
2. QDomCDATASection toCDATASection() const
3. QDomCharacterData toCharacterData() const
4. QDomComment toComment() const
5. QDomDocument toDocument() const
6. QDomDocumentFragment toDocumentFragment() const
7. QDomDocumentType toDocumentType() const
8. QDomElement toElement() const
9. QDomEntity toEntity() const
10. QDomEntityReference toEntityReference() const
11. QDomNotation toNotation() const
12. QDomProcessingInstruction toProcessingInstruction() const
13. QDomText toText() const
```

1. 将`QDomNode`转换为[`QDomAttr`](https://doc.qt.io/qt-6/qdomattr.html)。如果结点不是==属性（attribute）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
2. 将`QDomNode`转换为[`QDomCDATASection`](https://doc.qt.io/qt-6/qdomcdatasection.html)。如果结点不是==CDATA段（CDATA section）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
3. 将`QDomNode`转换为[`QDomCharacterData`](https://doc.qt.io/qt-6/qdomcharacterdata.html)。如果结点不是==字符数据结点（character data node）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
4. 将`QDomNode`转换为[`QDomComment`](https://doc.qt.io/qt-6/qdomcomment.html)。如果结点不是==注释（comment）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
5. 将`QDomNode`转换为[`QDomDocument`](https://doc.qt.io/qt-6/qdomdocument.html)。如果结点不是==文档（document）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
6. 将`QDomNode`转换为[`QDomDocumentFragment`](https://doc.qt.io/qt-6/qdomdocumentfragment.html)。如果结点不是==文档片段（document fragment）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
7. 将`QDomNode`转换为[`QDomDocumentType`](https://doc.qt.io/qt-6/qdomdocumenttype.html)。如果结点不是==文档类型（document type）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
8. 将`QDomNode`转换为[`QDomElement`](https://doc.qt.io/qt-6/qdomelement.html)。如果结点不是==元素（element）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
9. 将`QDomNode`转换为[`QDomEntity`](https://doc.qt.io/qt-6/qdomentity.html)。如果结点不是==实体（entity）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
10. 将`QDomNode`转换为[`QDomEntityReference`](https://doc.qt.io/qt-6/qdomentityreference.html)。如果结点不是==实体引用（entity reference）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
11. 将`QDomNode`转换为[`QDomNotation`](https://doc.qt.io/qt-6/qdomnotation.html)。如果结点不是==符号（notation）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
12. 将`QDomNode`转换为[`QDomProcessingInstruction`](https://doc.qt.io/qt-6/qdomprocessinginstruction.html)。如果结点不是==处理指令（processing instruction）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。
13. 将`QDomNode`转换为[`QDomText`](https://doc.qt.io/qt-6/qdomtext.html)。如果结点不是==文本（text）==，则返回的对象为 [null](https://doc.qt.io/qt-6/qdomnode.html#isNull)。

##### 是否为某结点类型

```cpp
1. bool isAttr() const
2. bool isCDATASection() const
3. bool isCharacterData() const
4. bool isComment() const
5. bool isDocument() const
6. bool isDocumentFragment() const
7. bool isDocumentType() const
8. bool isElement() const
9. bool isEntity() const
10. bool isEntityReference() const
11. bool isNotation() const
12. bool isProcessingInstruction() const
13. bool isText() const
```

以`[1]`为例，其余同理：

如果结点是一个属性（attribute），返回`true`；否则返回`false`。

但是返回`true`并不意味着该对象是一个`QDomAttribute`，需要调用`toAttribute()`来获取`QDomAttribute`。

### 子结点

##### `QDomNode firstChild() const`

返回结点的 firstChild。如果没有子结点，则返回[空结点](https://doc.qt.io/qt-6/qdomnode.html#isNull)。

修改返回的结点，会同时修改文档树中的对应结点。

##### `QDomNode lastChild() const`

返回结点的 lastChild。如果没有子结点，则返回[空结点](https://doc.qt.io/qt-6/qdomnode.html#isNull)。

修改返回的结点，会同时修改文档树中的对应结点。

##### `QDomNodeList childNodes() const`

Returns a list of all direct child nodes.

==最常用于`QDomElement`对象上调用==。

例如，若 XML 文档如下：

```xml
<body>
<h1>Heading</h1>
<p>Hello <b>you</b></p>
</body>
```

则 "body" 元素的子结点列表将包含由`<h1>`标签创建的结点和`<p>`标签创建的结点。

列表中的结点不是拷贝的，因此==修改列表中的结点会同时修改对应的原结点==。

相关内容：[QDomNodeList](https://doc.qt.io/qt-6/qdomnodelist.html)类

##### `bool hasChildNodes() const`

判断结点是否有子结点。

### 兄弟结点

##### `QDomNode nextSibling() const`

返回文档树中的下一个兄弟结点。修改返回的结点，会同时修改文档树中的对应结点。

若 XML 文档如下：

```xml
<h1>Heading</h1>
<p>The text...</p>
<h2>Next heading</h2>
```

且该`QDomNode`代表`<p>`标签，则`nextSibling()`将返回代表`<h2>`标签的结点。

##### `QDomNode previousSibling() const`

返回文档树中的前一个兄弟结点。修改返回的结点，会同时修改文档树中的对应结点。

若 XML 文档如下：

```xml
<h1>Heading</h1>
<p>The text...</p>
<h2>Next heading</h2>
```

且该`QDomNode`代表`<p>`标签，则`previousSibling()`将返回代表`<h1>`标签的结点。

### 筛选结点

##### `QDomElement firstChildElement(const QString &tagName = QString(), const QString &namespaceURI = QString()) const`

返回首个标签名为 *tagName* 、名称空间 URI 为 *namespaceURI* 的**元素类型子结点**。

> 注意：只遍历**直接**子结点。

如果 *tagName* 为空，返回首个名称空间URI为 *namespaceURI* 的子结点；如果 *namespaceURI* 为空，返回首个标签名为 *tagName* 的子结点。==如果两参数都为空，返回首个元素类型子结点==。

如果不存在这样的子结点，返回[空元素](https://doc.qt.io/qt-6/qdomnode.html#isNull)。

##### `QDomElement lastChildElement(const QString &tagName = QString(), const QString &namespaceURI = QString()) const`

类比`firstChildElement()`。

##### `QDomElement previousSiblingElement(const QString &tagName = QString(), const QString &namespaceURI = QString()) const`

类比`firstChildElement()`。

##### `QDomElement nextSiblingElement(const QString &tagName = QString(), const QString &namespaceURI = QString()) const`

类比`firstChildElement()`。

### 属性

##### `QDomNamedNodeMap attributes() const`

返回一个包含所有属性的**具名结点映射**（named node map）。==只有`QDomElement`才有属性==。

==修改 map 中的属性会同步反映到该`QDomNode`的属性上==。

##### `bool hasAttributes() const`

判断结点是否有属性（attribute）。

### 值

##### `QString nodeValue() const`

返回结点的值。

“值”的含义取决于派生类：

| Name                                                                                 | Meaning                                |
| ------------------------------------------------------------------------------------ | -------------------------------------- |
| [`QDomAttr`](https://doc.qt.io/qt-6/qdomattr.html)                                   | The attribute value                    |
| [`QDomCDATASection`](https://doc.qt.io/qt-6/qdomcdatasection.html)                   | The content of the CDATA section       |
| [`QDomComment`](https://doc.qt.io/qt-6/qdomcomment.html)                             | The comment                            |
| [`QDomProcessingInstruction`](https://doc.qt.io/qt-6/qdomprocessinginstruction.html) | The data of the processing instruction |
| [`QDomText`](https://doc.qt.io/qt-6/qdomtext.html)                                   | The text                               |

其他派生类不具有结点值，将返回空字符串。

##### `void setNodeValue(const QString &value)`

将结点的值设为 *value*。

### 名称

##### `QString nodeName() const`

返回结点的名称。

“名称”的含义取决于派生类：

|Name|Meaning|
|---|---|
|[QDomAttr](https://doc.qt.io/qt-6/qdomattr.html)|The name of the attribute|
|[QDomCDATASection](https://doc.qt.io/qt-6/qdomcdatasection.html)|The string "#cdata-section"|
|[QDomComment](https://doc.qt.io/qt-6/qdomcomment.html)|The string "#comment"|
|[QDomDocument](https://doc.qt.io/qt-6/qdomdocument.html)|The string "#document"|
|[QDomDocumentFragment](https://doc.qt.io/qt-6/qdomdocumentfragment.html)|The string "#document-fragment"|
|[QDomDocumentType](https://doc.qt.io/qt-6/qdomdocumenttype.html)|The name of the document type|
|[QDomElement](https://doc.qt.io/qt-6/qdomelement.html)|The tag name|
|[QDomEntity](https://doc.qt.io/qt-6/qdomentity.html)|The name of the entity|
|[QDomEntityReference](https://doc.qt.io/qt-6/qdomentityreference.html)|The name of the referenced entity|
|[QDomNotation](https://doc.qt.io/qt-6/qdomnotation.html)|The name of the notation|
|[QDomProcessingInstruction](https://doc.qt.io/qt-6/qdomprocessinginstruction.html)|The target of the processing instruction|
|[QDomText](https://doc.qt.io/qt-6/qdomtext.html)|The string "#text"|

注意：在处理**元素**和**属性**结点的名称时，该函数不考虑名称空间。因此，返回的名称会包含名称空间前缀（if any）。要获取**元素**或**属性**结点的名称，请使用[`localName()`](https://doc.qt.io/qt-6/qdomnode.html#localName)；要获取名称空间前缀，请使用[`namespaceURI()`](https://doc.qt.io/qt-6/qdomnode.html#namespaceURI)。

### 其他

##### `bool isNull() const`

如果结点为空（即没有类型或内容），返回`true`；否则返回`false`。

##### `bool isSupported(const QString &feature, const QString &version) const`

如果 **DOM 实现** 实现了特性 *feature*，且该版本 *version* 中的结点支持此特性，返回`true`；否则返回`false`。

##### `void clear()`

将结点转为空结点；如果它之前不是空结点，其类型和内容会被删除。

