指针：指针变量相对应的内存空间存储的值恰好是某个内存地址。

//
//  pointer.cpp
//  testcpp
//
//  Created by 石迎 on 2019/2/27.
//  Copyright © 2019 石迎. All rights reserved.
//
#include <iostream>
#include <stdio.h>
using namespace std;

int main()
{
    int x = 5;
    int *p = &x;
    cout<<"x地址："<<&x<<"    *p:"<<*p <<"  p:"<<p<<endl;
}

输出：
x地址：0x7ffeefbff5ac    *p:5  p:0x7ffeefbff5ac

    int y = 5;
    int &p1 = y;
    int p2 = y;
    cout<<"y地址："<<&y<<"    &p1:"<<&p1 <<"  p1:"<<p1<<"    &p2:"<<&p2 <<"  p2:"<<p2<<endl;
    
输出：
y地址：0x7ffeefbff58c    &p1:0x7ffeefbff58c  p1:5    &p2:0x7ffeefbff57c  p2:5
1. p1和指针p的差别在于p1内存地址不可以变化。但是指针p可以重新指向其他地址。
2. p1可不需要取地址符来赋值，可以直接通过变量名

二级指针：存放指针的指针
#include <iostream>
#include <stdio.h>
using namespace std;

int main()
{
    char a[] = "string";
    char *p = a;
    char *p1 = p;
    cout<<"P:"<<p<<"  p1:"<<p1<<endl;
    cout<<"&P:"<<&p<<"  &p1:"<<&p1<<endl;
}
输出：
P:string  p1:string
&P:0x7ffeefbff540  &p1:0x7ffeefbff538
p1是一个指针变量,其中存放着p的地址，但是p1也要占空间的啊，所以p1也有地址，但是p内存中存放的是a的地址。所以p和p1的内容相同，但是内存地址不同。

函数指针：

函数指针指向某种特定类型，函数的类型由其参数及返回类型共同决定，与函数名无关。举例如下：

int add(int nLeft,int nRight);//函数定义  
 该函数类型为int(int,int),要想声明一个指向该类函数的指针，只需用指针替换函数名即可：

int (*pf)(int,int);//未初始化  
  则pf可指向int(int,int)类型的函数。pf前面有*，说明pf是指针，右侧是形参列表，表示pf指向的是函数，左侧为int，说明pf指向的函数返回值为int。则pf可指向int(int,int)类型的函数。而add类型为int(int,int),则pf可指向add函数。

pf = add;//通过赋值使得函数指针指向某具体函数  
  注意：*pf两端的括号必不可少，否则若为如下定义：

int *pf(int,int);//此时pf是一个返回值为int*的函数，而非函数指针 

函数类型为形参，自动调用函数的函数指针；如果函数指针为形参，传入对应函数指针。

