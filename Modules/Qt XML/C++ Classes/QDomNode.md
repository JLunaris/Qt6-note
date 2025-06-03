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

以1为例，其余同理：

如果结点是一个属性（attribute），返回`true`；否则返回`false`。

但是返回`true`并不意味着该对象是一个`QDomAttribute`，需要调用`toAttribute()`来获取`QDomAttribute`。

### 其他

##### `bool isNull() const`

如果结点为空（即没有类型或内容），返回`true`；否则返回`false`。

##### `bool isSupported(const QString &feature, const QString &version) const`

如果 **DOM 实现** 实现了特性 *feature*，且该版本 *version* 中的结点支持此特性，返回`true`；否则返回`false`。
