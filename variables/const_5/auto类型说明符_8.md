#### auto类型说明符
- auto类型说明符，能让编译器代替我们去分析表达式所属的类型
- auto让编译器通过初始值来推算变量的类型
- auto定义的变量必须要有初始值

```c++
 auto i = 0, *p = &i;     //正确,i是整数，p是整型指针
 auto sz = 0, pi = 3.14;  //错误：sz和pi的类型不一致
```
#### 复合类型、常量和auto的关系
- 编译器会适当地改变结果类型使其更符合初始化规则
- 编译器以引用对象的类型作为auto的类型
```c++
 int i = 0, &r = i;
 auto a = r;   //a是一个整数（r是i的别名，而i是一个整数）
```
- auto一般会忽略顶层const,同时底层const则会保留下来
```c++
const int ci = i, &cr = ci;
auto b = ci;     //b是一个整数（ci的顶层const特性被忽略掉了）
auto c = cr;     //c是一个整数 （cr是ci的别名，ci本身是一个顶层const）
auto d = &i;     //d是一个整型指针（整数的地址就是指向整数的指针）
auto e = &ci;    //e是一个指向整数常量的指针（对常量对象取地址是一种底层const）
```

- 如果希望推断出来的auto类型是一个顶层const,需要明确指出：
```c++
int i = 0, &r = i;
const int ci = i, &cr = ci;
const auto f = ci;   //ci的推演类型是int,f是const int
```

- 还可以将引用类型设为auto,此时原来的初始化规则依然适用
- 设置类型为auto的引用时，初始值中的顶层常量属性依然保留。如果初始值绑定一个引用，则此时常量就不是顶层常量了。
```c++
const int ci = i, &cr = ci;
auto &g = ci;         //g是一个整型常量引用，绑定到ci
auto &h = 42;         //错误:不能为非常量引用绑定字面值
const auto &j = 42;   //正确：可以为常量引用绑定字面值
```

- 要在一条语句中定义多个变量，符号&和*只是属于某个声明符，而非基本数据类型的一部分，因此初始值必须是同一类型
```c++

int i = 0, &r = i;
const int ci = i, &cr = ci;
auto k = ci, &l = i;     //k是整数，l是整型引用
auto &m = ci, *p = &ci;  //m是对整型常量的引用，p是指向整型常量的指针
auto &n = i, *p2 = &ci;  //错误：i类型是int而&ci的类型是const int
```