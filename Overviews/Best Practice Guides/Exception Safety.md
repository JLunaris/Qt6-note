https://doc.qt.io/qt-6/exceptionsafety.html

警告：异常安全特性尚未完全实现！一些常见的场景能正常工作，但某些类仍可能发生内存泄漏甚至崩溃。

==Qt 本身不会抛出异常==，而是使用**错误码**来代替。此外，某些类提供用户可见的错误信息，如[`QIODevice::errorString()`](https://doc.qt.io/qt-6/qiodevice.html#errorString)、[`QSqlQuery::lastError()`](https://doc.qt.io/qt-6/qsqlquery.html#lastError)。这种设计出于历史和实际原因——启用异常机制会使库的体积增加超过 20%。