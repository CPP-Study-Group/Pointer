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

