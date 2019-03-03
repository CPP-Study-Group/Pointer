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

#include <iostream>
#include <stdio.h>
using namespace std;

//声明一个函数指针，char表示指向的函数返回类型是char类型的(int)表示函数的参数类型，*pFun是函数指针的名字
char (*pFun)(int);
//声明一个函数，返回类型为char,参数为int类型
char glFun(int a)
{
    return a;
    
}

int main()
{
    pFun = glFun; //给函数指针赋值，将函数的地址赋给函数指针
    int prama =  (*pFun)(2); //通过函数指针调用函数
    cout<<"指向指针的函数："<<prama<<endl;
}
输出：
指向指针的函数：2

##指针函数
###定义
指针函数，简单的来说，就是一个返回指针的函数，其本质是一个函数，而该函数的返回值是一个指针。
声明格式为：*类型标识符 函数名(参数表)

#include <iostream>
#include <stdio.h>
using namespace std;

//声明一个函数，返回类型为char,参数为int类型
char* glFun(char* a)
{
    return a;
}

int main()
{

    char* aa = "aaa";
    char *p = glFun(aa);
    cout<<"指针函数："<<p<<endl;
}
输出：
指针函数：aaa


野指针：
所谓的野指针指的是一个指针变量指向了不可使用的内存空间。

产生野指针三个原因：

（1）指针变量创建时候没有被初始化：任何指针变量在创建的时候，不会自动成为NULL指针，它的默认值是随机的，因此该指针就会成为一个野指针，可能指向一块不可使用的内存空间。

         例如char *p;  这样创建一个指针p，指向一个随机的内存地址空间

         所以指针在创建的时候要被初始化，可以讲其初始化为NULL，或指向合法的内存空间

         比如 char *p = NULL ; 或 char *p = new char; //这个时候p就不会是一个野指针

（2）delete或free指针之后没有把指针设置为NULL：delete和free只是把指针所指的内存空间释放掉，而没有对指针本身进行释放。

         比如char *p = new char(4) ; delete[] p; //这时候指针p所指的内存空间被释放，但是指针p本身不为空，但是指针p所指向的内存空间已经不能使用，造成了野指针。正确的做法是及时的把指针p赋值为NULL

         例如下面这个程序

char *p = (char *)malloc(100);

strcpy(*p, "hello");

free(p);

if(p != NULL){

      printf("not NULL\n");

}

结果输出”not NULL“，验证上面的结论，应该在free之后马上把p = NULL。

（3）指针操作超过了指向内存空间的作用范围：当指针越界之后也会变成一个野指针


 空指针

空指针是一种特殊的指针，表示当前这个指针变量处于空闲状态，没有指向任何有意义的内存空间。

C++中用NULL来表示空指针，NULL是一个符号常量值为0。


智能指针：

       C++程序设计中使用堆内存是非常频繁的操作，堆内存的申请和释放都由程序员自己管理。程序员自己管理堆内存可以提高了程序的效率，但是整体来说堆内存的管理是麻烦的，C++11中引入了智能指针的概念，方便管理堆内存。使用普通指针，容易造成堆内存泄露（忘记释放），二次释放，程序发生异常时内存泄露等问题等，使用智能指针能更好的管理堆内存。

理解智能指针需要从下面三个层次：

1. 从较浅的层面看，智能指针是利用了一种叫做RAII（资源获取即初始化）的技术对普通的指针进行封装，这使得智能指针实质是一个对象，行为表现的却像一个指针。
2. 智能指针的作用是防止忘记调用delete释放内存和程序异常的进入catch块忘记释放内存。另外指针的释放时机也是非常有考究的，多次释放同一个指针会造成程序崩溃，这些都可以通过智能指针来解决。
3. 智能指针还有一个作用是把值语义转换成引用语义。C++和Java有一处最大的区别在于语义不同，
在Java里面下列代码：
　　Animal a = new Animal();
　　Animal b = a;
   这里其实只生成了一个对象，a和b仅仅是把持对象的引用而已。
   但在C++中不是这样，
    Animal a;
    Animal b = a;
    这里却是就是生成了两个对象。
智能指针在C++11版本之后提供，包含在头文件<memory>中，shared_ptr、unique_ptr、weak_pt
    
shared_ptr多个指针指向相同的对象。shared_ptr使用引用计数，每一个shared_ptr的拷贝都指向相同的内存。每使用他一次，内部的引用计数加1，每析构一次，内部的引用计数减1，减为0时，自动删除所指向的堆内存。shared_ptr内部的引用计数是线程安全的，但是对象的读取需要加锁。注意避免循环引用

#include <iostream>
#include <memory>

int main() {
    {
        int a = 10;
        std::shared_ptr<int> ptra = std::make_shared<int>(a);
        std::shared_ptr<int> ptra2(ptra); //copy
        std::cout << ptra.use_count() << std::endl;
        
        int b = 20;
        //std::shared_ptr<int> ptrb = pb;  //error
        std::shared_ptr<int> ptrb = std::make_shared<int>(b);
        ptra2 = ptrb; //assign
        
        std::cout << ptra.use_count() << std::endl;
        std::cout << ptrb.use_count() << std::endl;
    }
}
输出：
2
1
2

unique_ptr“唯一”拥有其所指对象，同一时刻只能有一个unique_ptr指向给定对象（通过禁止拷贝语义、只有移动语义来实现）。

#include <iostream>
#include <memory>

int main() {
    {
        std::unique_ptr<int> uptr(new int(10));  //绑定动态对象
        //std::unique_ptr<int> uptr2 = uptr;  //不能賦值
        //std::unique_ptr<int> uptr2(uptr);  //不能拷貝
        std::unique_ptr<int> uptr2 = std::move(uptr); //轉換所有權
        uptr2.release(); //释放所有权
    }
    //超過uptr的作用域，內存釋放
}
