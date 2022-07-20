#### constexpr变量
- constexpr类型由编译器来验证变量是否是一个常量表达式
- 声明为constexpr的变量一定是一个常量，并且必须用常量表示初始化
- 一般来说，如果你认定变量是一个常量表达式，那就把它声明成constexpr类型  
  ```c++
    constexpr int mf = 20;         //20是常量表达式
    constexpr int limit = mf + 1;  //mf + 1是常量表达式
    constexpr int sz = size();     //只有当size是一个constexpr函数时这才是一条正确的声明语句

    int i = 10;
    constexpr int a = i; //错误，变量i不是常量


    const int i = 10;
    constexpr int b = i; //正确，变量i是一个常量
  ```
#### 指针和constexpr
- 字面值类型可以被定义成constexpr
- 指针和引用都可以被定义成constexpr,但他们值却有严格的限制
   - 一个constexpr指针的初始值必须是**nullptr**或者0,或是存储于某个固定地址中的对象
   - 函数体内的变量一般来说并非存放在固定地址中，因此constexpr指针不能指向这样的变量
   - 定义在函数体之外的对象其地址固定不变，能用来初始化constexpr指针
- 在constexpr中如果定义了一个指针，那么constexpr会把它所定义的对象置为顶层const(常量指针)
  ```c++
  const int *p = nullptr;       //p是指向整型常量的指针
  constexpr int *q = nullptr;   //q是指向整数的常量指针
  ```

- constexpr指针既可以指向常量也可以指向一个非常量
  ```c++
  constexpr int *np = nullptr; //np是一个整数的常量指针，其值为空
  int j = 0;
  constexpr int i  = 42;     //i的类型是整型常量
  //i和j都必须定义在函数体之外
  constexpr const int *p = &i;   //p是常量指针，指向整型常量i
  constexpr int *p1 = &j;        //p1是常量指针，指向整数j
  ```