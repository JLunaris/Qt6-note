https://doc.qt.io/qt-6/moc.html

# Limitations

`moc`不会处理 C++ 的全部。主要问题是==类模板不能有`Q_OBJECT`宏==。例如：

```cpp
class SomeTemplate<int> : public QFrame
{
	Q_OBJECT
	...

signals:
	void mySignal(int);
}
```

以下构造是非法的，它们都有我们认为更好的替代方案，因此消除这些限制并不是我们的优先事项。

### 嵌套类不能有信号或槽

```cpp
class A
{
public:
	class B
	{
		Q_OBJECT

	public slots: // WRONG
		void b();
	};
};
```

### 信号/槽的返回类型不能是引用

信号和槽可以有返回类型，但==返回引用的信号或槽会被视为返回`void`==。

