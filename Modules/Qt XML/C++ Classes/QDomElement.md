https://doc.qt.io/qt-6/qdomelement.html

`QDomElement`表示 DOM 树中的一个元素。

- Inherits：[[QDomNode]]
- Cmake：
```cmake
find_package(Qt6 REQUIRED COMPONENTS Xml)
target_link_libraries(mytarget PRIVATE Qt6::Xml)
```

注意：该类中的所有函数都是[可重入的](https://doc.qt.io/qt-6/threads-reentrancy.html)。

# Public Functions

### 标签名

##### `QString tagName() const`

返回该元素的标签名。对于下列的 XML 文件：

```xml
<img src="myimg.png">
```

该函数返回`"img"`。

### 属性

##### `QString attribute(const QString &name, const QString &defValue = QString()) const`

返回名为 *name* 的属性的属性值。如果属性不存在，返回 *defValue*。

对于下列元素结点：

```xml
<img src="myimg.png">
```

`attribute("src")`返回`"myimg.png"`。

##### `QDomAttr attributeNode(const QString &name)`

返回名为 *name* 的属性（[`QDomAttr`](https://doc.qt.io/qt-6/qdomattr.html)型）。如果属性不存在，返回[空属性](https://doc.qt.io/qt-6/qdomnode.html#isNull)。

##### `QDomNamedNodeMap attributes() const`

返回[`QDomNamedNodeMap`](https://doc.qt.io/qt-6/qdomnamednodemap.html)，包含该元素的全部属性。