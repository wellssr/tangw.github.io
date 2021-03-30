### explicit
C++中，一个参数的构造函数(或者除了第一个参数外其余参数都有缺省值的多参构造函数)，承担了两个角色。

用于构建单参数的类对象
隐含的类型转换操作符
声明为explicit的构造函数不能在隐式转换中使用，只能显示调用，去构造一个类对象。

explicit关键字只对有一个参数的类构造函数有效, 如果类构造函数参数大于或等于两个时, 是不会产生隐式转换的, 所以explicit关键字也就无效了

但是将拷贝构造函数声明成explicit并不是良好的设计，一般只将有单个参数的constructor声明为explicit，而copy constructor不要声明为explicit.


```C++
#include <iostream>
using namespace std;

class Point {
public:
    int x, y;
    Point(int x = 0, int y = 0)
        : x(x), y(y) {}
};

void displayPoint(const Point& p) 
{
    cout << "(" << p.x << "," 
         << p.y << ")" << endl;
}

int main()
{
    displayPoint(1);
    Point p = 1;
}
```
们定义了一个再简单不过的Point类, 它的构造函数使用了默认参数. 这时主函数里的两句话都会触发该构造函数的隐式调用. (如果构造函数不使用默认参数, 会在编译时报错)

显然, 函数displayPoint需要的是Point类型的参数, 而我们传入的是一个int, 这个程序却能成功运行, 就是因为这隐式调用. 另外说一句, 在对象刚刚定义时, 即使你使用的是赋值操作符=, 也是会调用构造函数, 而不是重载的operator=运算符.

这样悄悄发生的事情, 有时可以带来便利, 而有时却会带来意想不到的后果. explicit关键字用来避免这样的情况发生.
1.指定构造函数或转换函数 (C++11起)为显式, 即它不能用于隐式转换和复制初始化.
2.explicit 指定符可以与常量表达式一同使用. 函数若且唯若该常量表达式求值为 true 才为显式. (C++20起)

```C++
// 加了explicit之后的代码
#include <iostream>
using namespace std;

class Point {
public:
    int x, y;
    explicit Point(int x = 0, int y = 0)
        : x(x), y(y) {}
};

void displayPoint(const Point& p) 
{
    cout << "(" << p.x << "," 
         << p.y << ")" << endl;
}

int main()
{
    displayPoint(Point(1));
    Point p(1);
}
```
