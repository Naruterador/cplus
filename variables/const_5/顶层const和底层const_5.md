## 如何区分顶层const和底层const
- 指针如果添加const修饰符时有两种情况：
- 指向常量的指针：
   - 代表不能改变其指向内容的指针。声明时const可以放在类型名前后都可，拿int类型来说，声明时：const int和int const 是等价的。
   - 声明指向常量的指针也就是 底层const，下面举一个例子：

```c++
int num_a = 1;
int const  *p_a = &num_a; //底层const
//*p_a = 2;  //错误，指向“常量”的指针不能改变所指的对象
#注意：指向“常量”的指针不代表它所指向的内容一定是常量，只是代表不能通过解引用符（操作符*）来改变它所指向的内容。
#上例中指针p_a指向的内容就不是常量，可以通过赋值语句：num_a=2;  来改变它所指向的内容。
```
- 指针常量：
   - 代表指针本身是常量，声明时必须初始化，之后它存储的地址值就不能再改变。
   - 声明时const必须放在指针符号*后面，即：*const 。声明常量指针就是顶层const，下面举一个例子：

```c++
int num_b = 2;
int *const p_b = &num_b; //顶层const
//p_b = &num_a;  //错误，常量指针不能改变存储的地址值
#其实顶层const和底层const很简单， 一个指针本身添加const限定符就是顶层const，而指针所指的对象添加const限定符就是底层const。 
```

## 区分顶层const和底层const的作用
- 为什么要区分顶层const和底层const呢，根据C++primer的解释，区分后有两个作用。
   - 执行对象拷贝时有限制，常量的底层const不能赋值给非常量的底层const。
   - 也就是说，你只要能正确区分顶层const和底层const，你就能避免这样的赋值错误。下面举一个例子：

```c++
int num_c = 3;
const int *p_c = &num_c;  //p_c为底层const的指针
//int *p_d = p_c;  //错误，不能将底层const指针赋值给非底层const指针
const int *p_d = p_c; //正确，可以将底层const指针复制给底层const指针
```

- 使用命名的强制类型转换函数const_cast时，需要能够分辨底层const和顶层const，因为const_cast只能改变运算对象的底层const。下面举一个例子：

```c++
int num_e = 4;
const int *p_e = &num_e;
//*p_e = 5;  //错误，不能改变底层const指针指向的内容
int *p_f = const_cast<int *>(p_e);  //正确，const_cast可以改变运算对象的底层const。但是使用时一定要知道num_e不是const的类型。
*p_f = 5;  //正确，非顶层const指针可以改变指向的内容
cout << num_e;  //输出5
```


- 顶层const可以赋值给非常量指针对象
```c++
int num_e = 4;
int *const p_e = &num_e;
int * p_d = p_e;   //正确：可以将顶层const的值赋值给非常量指针
*p_e = 10;   //正确：可以使用解引符对参数进行修改


int num_c = 10;
const int *const p_l = &num_c; //靠右的const是顶层，靠左的是底层
int *p = p_l //错误: p_l包含底层const的定义，而p没有
*p_l = 100; //错误:不能修改
const int *q = p_l; //正确：q和p_l都是底层const
int *const pp = p_l; //错误:pp是顶层const，而p_l是底层const
```

- 底层const不能赋值给顶层const
```c++
int i = 10;
int const *ppp = &i;
int *const q = ppp; //错误
```

- 顶层const能赋值给底层const
```c++
int i = 10;
int *const q = &i;
int const *ptr = q;
```

- 用于声明和引用的const都是底层const
```c++
const int &r = i;  //底层const
```

> 总结:
>> **顶层const可以赋给非常量，底层const**
>> **底层const不能赋值给非底层const**      


## 练习
- 说了这么多，应该练习一下，const int *const *const *pppi 是顶层const还是底层const？
   - 答案当然是底层const，因为int前面const限定符，而最后一个*后面没有const限定符。看最后一个例子：

```c++
const int a = 1;  
//int * pi = &a;  //错误，&a是底层const，不能赋值给非底层const 
const int * pi = &a; //正确，&a是底层const，可以赋值给底层const
const int *const *const ppi = &pi  //即是底层const，也是顶层const
const int  *const *const *pppi = &ppi; //底层const
```
