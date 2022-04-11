---
title: （C++）cout与printf的区别
date: 2022-03-05 14:23:32
categories: Computer Science
tags: C++
description: <div align = "center"><font size=3>C++的输入输出有2中不同的形式，针对这两种形式进行了归纳整理</font></div>
---

&emsp;&emsp;写在开头：cout与printf不仅在基本形式上有区别，在一些对于输出格式的控制上也有明显差异，下面将从两者的不同点出发，记录两者的一系列基本用法，方便理解。[^不同的还有许多，但目前水平有限，仅记录如下部分，之后再补充]

### Cout 与 Printf的基本区别

####  输出原理

- Cout
  - 使用重载：根据输出内容的类型重载不同类型的函数，所以可以输出包括自定义类型在内的多种类型。例如，在cout中，相当于有很多cout的同名函数，但它们有不同类型的函数：如int float char等，当输出类型为char类型时，调用参数为char函数。
  - 开辟缓冲区：定义每一个流对象时，系统会在内存中开辟一段缓冲区，用来暂存数据(系统内有多个缓冲区)。当收到endl时，cout行会进行换行，同时刷新缓冲区。Cout输出过程：先将输出字符放入缓冲区，然后输出屏幕。（当缓冲区满或者收到结束符时，会将缓冲区数据一并清空并在显示设备输出）

-  Printf
  - 类型由%d，%f等格式符规定；
  - 输出时没有缓冲区。

#### 打印速度

Cin，cout与scanf，printf打印效率后者更快，在做io或acm时多用后者。

#### 格式部分

`Cout：std：：cout<<”cout 输出”<<std::endl;`

`Printf: printf(“其他+%转换+其他”，参数)；`

### IO流控制

- 控制位数显示。基本用法如下`Iomanip、Fixed、scientific、setprecision（n)`：

  - 在一般表达下，前面的控制符仅表示amount的有效位数。

    `Cout<<setprecision（n）<<amount<<endl; `

  - 在固定表示的输出中，控制符表示amount的小数位数。

    `Cout<<fixed<<setprecision(n)<<amount<<endl;`

  - 在指数形式的输出中，控制符表示amount的小数位数。

    `Cout<<scientific<<setprecision(n)<<amount<<endl;`

```c++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
	double pi = 3.1415926525;
	cout << setprecision(8)<<pi << endl;			//3.1415927
	cout<<fixed << setprecision(8) << pi << endl;		//3.14159265
	cout << scientific << setprecision(8) << pi << endl;	//3.14159265e+00
	system("pause");
	return 0;
}
```



-  设置输出宽度，其基本用法如下`Setw`：

  - `Cout<<setw(n)<<amount<<endl;`

  - Setw间隔不保留其效力，其他控制符保留一直保留效果。

    ​          	`Cout<<setw(n)<<amount_1<<amount_2<<endl;`

    后面的amoutn_2的输出宽度为默认值0，即按输出数值的表示宽度输出。

  - 如果宽度比原字符少，则按原字符宽度输出，若宽度比原字符大，则在前面用空格填充。

```C++
	double pi = 3.1415926525;			//浮点数有效位数默认为6位,小数点也算以为宽度
	cout << setw(7)<<pi << endl;			//3.14159
	cout<<setfill('*') << setw(8) << pi << endl;	//*3.14159
```



- 输出八进制、十进制和十六进制数，基本方式如下`oct、dec、hex`：

  - 八进制

    `Cout<<otc<<amout<<endl;`

  - 十进制

    `Cout<<dec<<amount<<endl;`

  - 十六进制

    `Cout<<hex<<amount<<endl;`

    特别地，十六进制控制大小写用uppercase

    `Cout<<hex<<uppercase<<amount<<endl;`

```C++
	int a = -34;					
	cout << oct << a << endl;	//37777777736				
	cout << dec << a << endl;	//-34
	cout << hex << a << endl;	//ffffffde
```



- 左右对齐输出，基本方式如下`Left right`：

  - 不同对齐方式的区别在于填充符号在左还是在👉

    `Cout<<left<<setw(n)<<amount<<endl;`

    `Cout<<right<<setw(n)<<amount<<endl;`

```c++
	double pi = 3.1415926525;				//默认位右对齐
	cout <<setfill('*')<<right<<setw(8) << pi << endl;	//*3.14159
	cout <<left << setw(8) << pi << endl;			//3.14159*
```





- 设置填充字符，基本方式如下`setfill`：

  - 前面setw默认用空格填充，可通过setfill（‘*‘）来代替默认填充符

    `Cout<<setfill('*')<<setw(n)<<amount<<endl;`

```c++
	double pi = 3.1415926525;			//注意单引号
	cout << setfill('*') << setw(9) << pi << endl;		//**3.14159
```



- 强制显示小数点和符号`Showpoint showpos`：

  - `Count<<showpoint<<amount<<endl;`

    `Count<<showpos<<amount<<endl;`

```c++
	float a = 1314;			//针对浮点数
	cout  <<setfill('*')<<setw(6) << a << endl;		//**1314
	cout  << showpoint<<setw(6)<< a << endl;		//1314.00
```

### Printf与scanf

&emsp;基本格式：printf("格式符+符号a.b"，输出数值)

#### 基本格式符a（整数）

&emsp;在整数中，前面的数值a一般理解为输出宽度，若为有符号整数也包括输出的负号，宽度小于原有长度时输出默认表示长度，宽度大于原有宽度时右对齐填充。

- %d 用来输出十进制整数，可以修饰长度，对齐方式，默认为右对齐。
- - %o 用来输出八进制；
  - %x/X 用来输出十六进制，x/X修饰输出时的大小写；
- %u 用以无符号十进制整数输出；

```C++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
	int a = -1314;			//默认右对齐
	printf("%6d\n", a);		// -1314
	printf("%o\n", a);		//37777775336
	printf("%x\n", a);		//fffffade
	printf("%X\n", a);		//FFFFFADE
	unsigned int b = -2;	//4294967294
	printf("%u\n", b);
	system("pause");
	return 0;
}
```



#### 基本格式符b（字符串）

- %s 字符串格式输出，可指定输出长度，对齐方式以及选择字符输出位数。

```C++
	printf("%5.3s", "hello,wrold");	//  hel
```



#### 基本格式符c（浮点数）

&emsp;前面的整数a表示输出位数，包括小数点，e，+等，小数表示保留小数位数。

- %f 小数方式输出，可指定输出长度，对齐方式，小数位数。
- %e 指数形式输出，可指定输出长度，对齐方式，小数位数。

```c++
#include <iostream>
#include <iomanip>
using namespace std;

int main(){
	//整数部分表示输出位数,小数部分表示小数保留位数；
	float pi = 3.1415926525;	//小数点、加号、e均算一位；
	printf("%8f\n", pi);	//3.141593
	printf("%8.4f\n", pi);	//  3.1416
	printf("%13e\n", pi);	// 3.141593e+00
	printf("%8.4e\n", pi);	//3.1416e+00
	system("pause");
	return 0;
}
```

### I/O流简述

\-   如C语言一样，C++语言中也没有输入输出语句。但C++编译系统带有一个面向对象的输入输出软件包，即I/O流类库。

\-   在C++中，将数据从一个对象到另一个对象的流动抽象为‘流‘。流是一种抽象，它负责在数据的生产者和消费者之间建立联系，并管理数据的流动。程序建立一个流对象，并制定这个流对象与某个文件对象建立连接，程序操作流对象，流对象通过文件系统对所连接的文件对象产生作用。

\-   I/O流类库中头文件<iostream>声明了8个预定义的流对象用来完成在标准设备上的输入输出操作：cin、cout、cerr、clog、wcin、wcout、wcerr、wclog。

\-   插入运算符（<<）和提取运算符（>>）是所有标准C++数据类型预先设计的，用于传送字节到一个输出流对象或从一个输入流对象提取字节。它们与预先定义的操纵符一起工作，可以控制输出格式。这些操纵符大多定义在ios_base类中和<iomanip>头文件中。
