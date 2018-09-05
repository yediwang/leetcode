# Type Inference

C++11中类型推导的实现之一就是重定义auto关键字，另一个实现是decltype。

## auto关键字

* auto使用必须被初始化，以下错误用法

```text
auto a;
a = 1;
```

这个意义上，auto并非一种类型声明，而是一个类型声明时的“占位符”，编译器在便已是亲会将suto替代为变量实际的类型。

* 我们可以使用valatile，pointer（\*），reference（&），rvalue reference（&&） 来修饰auto

```text
auto k = 5;
auto* pK = new auto(k);
auto** ppK = new auto(&k);
const auto n = 6;
```

* 函数和模板参数不能被声明为auto

```text
void MyFunction(auto parameter){} // no auto as method argument
template<auto T> // utter nonsense - not allowed
void Fun(T t){}
```

 但如果函数有一个尾随的返回类型时，auto是可以出现在函数声明中返回值位置。

```text
template<class T, class U> auto add(T t, U u) -> decltype(t + u)
{
    return t + u;
}
```

* auto不能自动推导成CV-qualifiers（constant & volatile qualifiers），除非被声明为引用类型

```text
const int i = 99;
auto j = i;       // j is int, rather than const int
j = 100           // Fine. As j is not constant
// Now let us try to have reference
auto& k = i;      // Now k is const int&
k = 100;          // Error. k is constant
// Similarly with volatile qualifer
```

*  auto会退化成指向数组的指针，除非被声明为引用

```text
int a[9];
auto j = a;
cout<<typeid(j).name()<<endl; // This will print int*
auto& k = a;
cout<<typeid(k).name()<<endl; // This will print int [9]
```

## decltype

decltype的类型推导并不是像auto一样是从**变量声明**的初始化表达式**获得**变量的类型，而是总是以一个普通表达式**作为参数**，**返回**该表达式的类型，而且decltype并不会对表达式进行求值。

*  如果e是一个变量或者类成员访问表达式，假设e的类型是T，那么的decltype\(e\)为T，decltype\(\(e\)\)为T&。 

```text
struct A { double x; };
const A* a = new A{0};
decltype(a->x) y;       // type of y is double
decltype((a->x)) z = y; // type of z is const double&,因为a一个常量对象指针
```

* 如果e是一个解引用操作，那么decltype\(e\)和decltype\(\(e\)\)均为T&。 

```text
int* aa=new int;
decltype(*aa) y=*aa;    //type of y is int&，解引用操作
```

* 否则decltype\(e\)与decltype\(\(e\)\)均为T。

```text
decltype(5) y;          //type of y is int
decltype((5)) y;        //type of y is int
const int&& RvalRef() { return 1; }
decltype ((RvalRef())) var = 1;  //type of var is const int&&
```

* 泛型编程中结合auto，用于追踪函数的返回值类型，这也是decltype的最大用途。

```text
template <typename _Tx, typename _Ty> auto multiply(_Tx x, _Ty y)->decltype(x*y)
{
    return x*y;
}
```



