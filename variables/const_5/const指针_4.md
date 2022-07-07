#### const指针
- 指针是对象而不是引用，所以允许把指针本身定义为常量
-  **常量指针(const pointer)** 必须初始化，一旦完成初始化，存放指针地址的那个值就不能再改变了
-  **把\*放在 const关键字前面说明指针是一个常量，这样书写隐含着一层意味，即不变的是指针本身的值，而非指向的那个值
```c++
int errNumb = 0;
int *const curErr = &errNumb;    //curErr将一个指向errNumb
const double pi = 3.1415926;
const double *const pip = &pi;   //pip是一个指向常量对象的常量指针

/*
先要弄清楚这些声明含义最行之有效的办法是从右向左阅读。
本例中，离curErr最近的符号是const,意味着curErr本身是一个常量对象，对象的类型由声明符确定其余部分确定；
声明符中的下一个符号是*,意思是curErr是一个常量指针；
最后，该声明语句的基本数据类型部分确定了常量指针指向的是一个int对象。
*/
```

- 指针本身是一个常量并不意味着不能通过指针修改其所指向对象的值，能否这样做完全依赖于所指对象的类型
```c++

const double pi = 3.1415926;
const double *const pip = &pi;   
*pip = 2.72;   //错误：pip是一个指向常量的指针
//pip是一个指向常量的指针，则不论是pip所指的对象值还是pip自己存储的那个地址都不能改变；

//curErr的指向是一个一般的非常量整数，那么就完全可以用curErr去修改errNumb的值:
int errNumb = 0;
int *const curErr = &errNumb;    //curErr将一个指向errNumb
*curErr = 0; //正确：把curErr所指的对象值重制
```