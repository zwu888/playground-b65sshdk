# 通过模板实现静态绑定
为了在编译时绑定，我们就需要放弃 C++ 的虚函数机制，而只是在基类和子类中实现同名的函数；同时，为了在编译时确定类型，我们就需要将子类的名字在编译时提前传给基类。因此，我们需要用到 C++ 的模板。

```C++ runnable
#include <iostream>
using namespace std;

template<typename T>
class Base {
    public:
        void show() const {
            static_cast<const T*>(this)->show();
        }
};
 
class Derived:public Base<Derived> {
    public:
        void show() const {
            cout << " shown in Derived class. " << endl;
        }
};

int main() {
    Derived d;
    Base<Derived> *b = &d;
    b->show();
    return 0;
}
