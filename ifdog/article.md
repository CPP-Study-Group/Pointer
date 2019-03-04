
-指针:
实际是一种数据类型，存放的是内存的地址。作为一种数据类型，指针可以参与一些运算，如算数运算、关系运算、赋值运算等。指针的主要作用实际就是实现内存的动态寻址，即将要访问的内存位置抽象成可运算的数据，以此来极大的提升操作数据的灵活性。
    例如：对指针的加减运算，实际是切换访问的目标数据位置，下面代码中使用指针的自增运算访问了数组中的不同元素。
```C
int nArray[3] = { 1, 2, 3 };   // 定义一个数组
int* pIndex = nArray;          // 将数组的起始地址赋值给指针pIndex
cout<<"指针指向的地址是："<<pIndex<<endl;       // 输出指针指向的地址
cout<<"指针所指向的数据的值是："<<*pIndex<<endl; // 输出这个位置上的数据

pIndex++;   // 对指针进行加运算，使其指向数组中的下一个值
cout<<"指针指向的地址是："<<pIndex<<endl;        // 输出指针指向的地址
cout<<"指针所指向的数据的值是："<<*pIndex<<endl;  // 输出数据 
```

-二级指针：
即指向指针的指针。指针作为内存数据，必然也可被指针寻址访问，这种指向指针的指针就称为二级指针,同理也存在n(n>0)级指针。
```c
void printArray(char **myarray)
{
	int i = 0;
	for (i = 0; i < 10; i++)
	{
		printf("%s\n", myarray[i]);
	}
}
```

-函数指针：
指针可指向任意内存地址，大部分指针都指向内存对象，而那些指向函数入口点的指针被称为函数指针。函数指针的类型实际是指向的函数的签名。（实际上似乎和C#的委托是一回事）
```C
bool LengthCompare(const string &,const string &)
bool (*pf)(const string&, const string&);//未初始化
pf=LengthCompare;
   
bool b1=pf("hello","goodbye");
bool b2=(*pf)("hello","goodbye");
bool b3=LengthCompare("hello","goodbye");
//三个等价调用
```

-智能指针：
在编程时使用指针（尤其是在堆内存中分配、释放空间时）管理内存很容易出错。因此很多c++的框架和库都定义了辅助管理内存的工具类。比较著名的就是c++11中引入的智能指针概念。智能指针实际上并不是语言特性，而是标准库提供的一个行为像指针的对象。（C++设计者似乎喜欢在标准库里塞东西，而不喜欢添加语言特性。） 智能指针的主要作用是帮助使用者确保内存会自动释放。
智能指针在C++11版本之后提供，包含在头文件<memory>中，shared_ptr、unique_ptr、weak_ptr。最常使用的是shared_ptr.多个指针指向相同的对象。它使用引用计数的方式来统计对象被引用的次数，当所有引用都失效时，自动删除对象并释放内存。（python,c#实际都是这个逻辑，只是释放时机不同）
```C
#include <iostream>
#include <memory>

int main() {
    {
        int a = 10;
        std::shared_ptr<int> ptra = std::make_shared<int>(a);
        std::shared_ptr<int> ptra2(ptra); //copy
        std::cout << ptra.use_count() << std::endl;
        int b = 20;
        int *pb = &a;
        //std::shared_ptr<int> ptrb = pb;  //error
        std::shared_ptr<int> ptrb = std::make_shared<int>(b);
        ptra2 = ptrb; //assign
        pb = ptrb.get(); //获取原始指针
        std::cout << ptra.use_count() << std::endl;
        std::cout << ptrb.use_count() << std::endl;
    }
}
```
     
unique_ptr“唯一”拥有其所指对象，同一时刻只能有一个unique_ptr指向给定对象（通过禁止拷贝语义、只有移动语义来实现）
```C
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
```
 weak_ptr可以从一个shared_ptr或者另一个weak_ptr对象构造，获得资源的观测权。但weak_ptr没有共享资源，它的构造不会引起指针引用计数的增加。使用weak_ptr的成员函数use_count()可以观测资源的引用计数，另一个成员函数expired()的功能等价于use_count()==0,但更快，表示被观测的资源(也就是shared_ptr的管理的资源)已经不复存在。weak_ptr可以使用一个非常重要的成员函数lock()从被观测的shared_ptr获得一个可用的shared_ptr对象， 从而操作资源。但当expired()==true的时候，lock()函数将返回一个存储空指针的shared_ptr。（不是特别理解它的用途但是似乎并不常用）。
```C
#include <iostream>
#include <memory>

int main() {
    {
        std::shared_ptr<int> sh_ptr = std::make_shared<int>(10);
        std::cout << sh_ptr.use_count() << std::endl;

        std::weak_ptr<int> wp(sh_ptr);
        std::cout << wp.use_count() << std::endl;

        if(!wp.expired()){
            std::shared_ptr<int> sh_ptr2 = wp.lock(); //get another shared_ptr
            *sh_ptr = 100;
            std::cout << wp.use_count() << std::endl;
        }
    }
    //delete memory
}
```

-空指针
指不指向任何地方的指针，要注意它与“未初始化的指针”不同。未初始化的指针通常是下面说的野指针，指向随机内存地址。
 
-野指针 
一个没有指无意义或是错误的内存区域的指针叫做野指针。野指针通常产生于未赋值的指针，或对应内存已释放而
指针仍然有效的指针。错误使用野指针会产生很多问题，例如对无权写入的内存区域进行写操作，会产生系统错误。
```C
char* p = NULL;  
char* str = (char*)malloc(1024);    
```

-零指针 （
似乎也是指空指针？）是指值为0的指针。C++标准规定，当一个指针的值为0时，就认为这个指针是空的。



  
