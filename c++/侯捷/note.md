# 上

## pass by reference

- **pass by value**和**pass by reference**区别
  - 前者是原封不动的copy一份存在栈中，大小不定
  - 后者相当于传递一个指针，大小固定4byte，速度更快

- 所有函数参数的值都尽量以reference传递，如果该参数不会变动，则可以用const reference
- 如果该参数会变动，如果希望变动影响到原参数，以reference传递，不希望影响到原参数，以value传递

![在描述](E:\Study\c++\··侯捷 - C++面向对象高级开发\note.assets\20201108145521367.png)

上图中执行的是复数的+=操作，指针ths指向的值会被改变，所以不能用reference传递；但是r中的值不会被改变，只是加在了ths上，所以可以用常引用来传递。

<img src="E:\Study\c++\··侯捷 - C++面向对象高级开发\note.assets\image-20220425161708448.png" alt="image-20220425161708448"  />

os的值会被改变，所以不加const

***

## return by reference

如果return的结果是已经存在有内存的东西，加&；

如果return的结果是用临时变量保存的，不加&；

- 如果要return的东西，运行这个函数之前已经有一块memory可以存放这个东西，则可以return by reference；
- 如果事先没有memory存放这个东西，那么只能传回一个local object。

比如：

```
complex&
___doap(complex* ths, const complex& r)
{
   ths->re += r.re;
   ths->im += r.im;
   return *ths;
}
```


该函数执行的是复数的+=操作，比如 “c1+=c2” ，那么在执行操作时结果就会存储在c1所在的区域内，所以可以return by reference，上述代码中返回的是complex&。

但是，比如：

```
complex
operator + (const complex& x, const complex& y)
{
   return complex ( real (x) + real (y), imag (x) + imag (y));
}
```

该函数执行的是复数的加法操作，比如"c1+c2"，但是在执行完该函数后不知道将加完之后的结果存储到哪里，所以只能返回一个local oblect，上述代码中返回的是complex对象。

***

## 内联函数inline

- 函数比较小时，加关键字inline可以提高函数执行效率。

- 在类内部定义的成员函数会自动成为inline，但我们习惯类内声明，类外定义加上inline。

- 加inline并不表示一定会成为内联函数，只是建议编译器这么做。

***

## 关于char*

字符串的本质是地址

char str[] = "hello"   等于  char* str = "hello"，

- str[]可以指定需要分配的内存，如str[10]分配10个单位内存，sizeof(str) = 10；
- char* str ，sizeof(str) = 4，这是个指针变量，只占四个字节

str = &str[0] ， 也等于 "hello" 的首地址

```
char* s;
s = "China";

其实真正的意义是 s = "China" = 0x3000;
printf("%s", s); 传给它的其实是s所保存的字符串的首地址，以'\0'结尾
```

***

## 虚指针和虚表

![image-20220427140237572](E:\Study\c++\··侯捷 - C++面向对象高级开发\note.assets\image-20220427140237572.png)

***

## Const

![image-20220427163800029](E:\Study\c++\··侯捷 - C++面向对象高级开发\note.assets\image-20220427163800029.png)

成员函数后面加const表示该函数不能改变class data数据

***

## New和Delete

new：先分配memory， 再调用ctor

delete：先调用dtor， 再释放memory