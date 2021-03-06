# STL_deque容器

### 一、deque简介

deque是“double-ended queue”的缩写，和vector一样都是STL的容器，deque是双端数组，而vector是单端的。

deque在接口上和vector非常相似，在许多操作的地方可以直接替换。

deque可以随机存取元素（支持索引值直接存取， 用[]操作符或at()方法）。

**deque头部和尾部添加或移除元素都非常快速。**但是在中部安插元素或移除元素比较费时。

#include \<deque> 

<img src="C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20210124101431485.png" alt="image-20210124101431485" style="zoom: 50%;" />

**原理：**

<img src="C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20210124101954916.png" alt="image-20210124101954916" style="zoom:50%;" />

Deque容器是连续的空间，至少逻辑上看来如此，连续现行空间总是令我们联想到array和vector,array无法成长，vector虽可成长，却只能向尾端成长，而且其成长其实是一个假象，事实上(1) 申请更大空间 (2)原数据复制新空间 (3)释放原空间 三步骤，如果不是vector每次配置新的空间时都留有余裕，其成长假象所带来的代价是非常昂贵的。

Deque是由一段一段的定量的连续空间构成。一旦有必要在deque前端或者尾端增加新的空间，便配置一段连续定量的空间，串接在deque的头端或者尾端。Deque最大的工作就是维护这些分段连续的内存空间的整体性的假象，并提供随机存取的接口，避开了重新配置空间，复制，释放的轮回，代价就是复杂的迭代器架构。

既然deque是分段连续内存空间，那么就必须有中央控制，维持整体连续的假象，数据结构的设计及迭代器的前进后退操作颇为繁琐。**Deque代码的实现远比vector或list都多得多**。

Deque采取一块所谓的map(注意，不是STL的map容器)作为主控，这里所谓的map是一小块连续的内存空间，其中每一个元素(此处成为一个结点)都是一个指针，指向另一段连续性内存空间，称作缓冲区。缓冲区才是deque的存储空间的主体。

**与vector的差异：**

Deque容器和vector容器最大的差异，一在于deque允许使用常数项时间对头端进行元素的插入和删除操作。二在于deque没有容量的概念，因为它是动态的以分段连续空间组合而成，随时可以增加一段新的空间并链接起来，换句话说，像vector那样，”旧空间不足而重新配置一块更大空间，然后复制元素，再释放旧空间”这样的事情在deque身上是不会发生的。也因此，deque没有必须要提供所谓的空间保留(reserve)功能。

虽然deque容器也提供了Random Access Iterator，但是它的迭代器并不是普通的指针，其复杂度和vector不是一个量级，这当然影响各个运算的层面。因此，除非有必要，**我们应该尽可能的使用vector，而不是deque**。**对deque进行的排序操作，为了最高效率，可将deque先完整的复制到一个vector中，对vector容器进行排序，再复制回deque**。

### 二、deque对象的构造

deque采用模板类实现，deque对象的默认构造形式：deque\<T> deqT; 

- deque \<int> deqInt;       //一个存放int的deque容器。

- deque \<float> deqFloat;   //一个存放float的deque容器。

- deque\<string> deqString;   //一个存放string的deque容器。           

-  //尖括号内还可以设置指针类型或自定义类型。 


deque(beg, end);//构造函数将[beg, end)区间中的元素拷贝给本身。

deque(n, elem);//构造函数将n个elem拷贝给本身。

deque(const deque &deq);//拷贝构造函数。

```cpp
	deque<int> d1;
	deque<int> d2(10, 5);
	deque<int> d3(d2.begin(), d2.end());
	deque<int> d4(d3);
```



### 三、deque末尾的添加移除操作

deque.push_back(elem);  //在容器尾部添加一个数据

deque.push_front(elem); //在容器头部插入一个数据

deque.pop_back();        //删除容器最后一个数据

deque.pop_front();      //删除容器第一个数据

```cpp
	deque<int> deqInt;

    deqInt.push_back(1);
    deqInt.push_back(3);
    deqInt.push_back(5);
    deqInt.push_back(7);

    deqInt.pop_front();
    deqInt.pop_front();

    deqInt.push_front(11);
    deqInt.push_front(13);

    deqInt.pop_back();

//deqInt { 13,11,5}
```



### 四、deque的数据存取

deque.at(idx); //返回索引idx所指的数据，如果idx越界，抛出out_of_range。

deque[idx]; //返回索引idx所指的数据，如果idx越界，不抛出异常，直接出错。

deque.front();  //返回第一个数据。

deque.back();  //返回最后一个数据     

```cpp
	deque<int> deqInt;
 
 	deqInt.push_back(1);
    deqInt.push_back(3);
    deqInt.push_back(5);
    deqInt.push_back(7);
    deqInt.push_back(9);

	int iA = deqInt.at(0);        //1
    int iB = deqInt[1];           //3
    deqInt.at(0) = 99;           //99
    deqInt[1] = 88;         //88
    int iFront = deqInt.front();    //99
    int iBack = deqInt.back();    //9
    deqInt.front() = 77;         //77
    deqInt.back() = 66;         //66
```



### 五、deque与迭代器

deque.begin(); //返回容器中第一个元素的迭代器。

deque.end(); //返回容器中最后一个元素之后的迭代器。

deque.rbegin(); //返回容器中倒数第一个元素的迭代器。

deque.rend();  //返回容器中倒数最后一个元素之后的迭代器。 

```cpp
	deque<int> deqInt;

    deqInt.push_back(1);
    deqInt.push_back(3);
	deqInt.push_back(5);
    deqInt.push_back(7);
    deqInt.push_back(9);

    for (deque<int>::iterator it=deqInt.begin(); it!=deqInt.end(); ++it)
    {
         cout << *it;
         cout << "";
     }
    // 1 3 5 7 9

	for (deque<int>::reverse_iterator rit=deqInt.rbegin(); rit!=deqInt.rend(); ++rit)
    {
	     cout << *rit;
         cout << "";
    }
    //9 7 5 3 1
```



### 六、deque的赋值

deque.assign(beg,end);  //将[beg, end)区间中的数据拷贝赋值给本身。注意该区间是左闭右开的区间。

deque.assign(n,elem); //将n个elem拷贝赋值给本身。

deque& operator=(const deque &deq);  //重载等号操作符

deque.swap(deq); // 将vec与本身的元素互换 

```cpp
	deque<int> deqIntA,deqIntB,deqIntC,deqIntD;

    deqIntA.push_back(1);
    deqIntA.push_back(3);
    deqIntA.push_back(5);
	deqIntA.push_back(7);
    deqIntA.push_back(9);

    deqIntB.assign(deqIntA.begin(),deqIntA.end());   // 1 3 5 7 9
   
    deqIntC.assign(5,8);                         //8 8 8 8 8

    deqIntD = deqIntA;                          //1 3 5 7 9

	deqIntC.swap(deqIntD);                      //互换
```



### 七、deque的大小

deque.size();     //返回容器中元素的个数

deque.empty();   //判断容器是否为空

deque.resize(num);  //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。

deque.resize(num, elem); //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。      

```cpp
	deque<int> deqIntA;

	deqIntA.push_back(1);
    deqIntA.push_back(3);
    deqIntA.push_back(5);

    int iSize = deqIntA.size(); //3

	if (!deqIntA.empty())
    {
        deqIntA.resize(5);      //1 3 5 0 0
        deqIntA.resize(7,1); 	//1 3 5 0 0 1 1
        deqIntA.resize(2);      //1 3
    }
```



### 八、deque的插入

deque.insert(pos,elem);  //在pos位置插入一个elem元素的拷贝，返回新数据的位置。

deque.insert(pos,n,elem);  //在pos位置插入n个elem数据，无返回值。

deque.insert(pos,beg,end);  //在pos位置插入[beg,end)区间的数据，无返回值。 

```cpp
	deque<int> deqA;
    deque<int> deqB;

    deqA.push_back(1);
	deqA.push_back(3);
    deqA.push_back(5);
	deqA.push_back(7);
    deqA.push_back(9);

	deqB.push_back(2);
    deqB.push_back(4);
    deqB.push_back(6);
    deqB.push_back(8);
   
    deqA.insert(deqA.begin(), 11);        //{11, 1, 3, 5, 7, 9}
    deqA.insert(deqA.begin()+1,2,33);     //{11,33,33,1,3,5,7,9}
    deqA.insert(deqA.begin() , deqB.begin() , deqB.end() ); //{2,4,6,8,11,33,33,1,3,5,7,9}
```



### 九、deque的删除

deque.clear();  //移除容器的所有数据

deque.erase(beg,end); //删除[beg,end)区间的数据，返回下一个数据的位置。

deque.erase(pos);  //删除pos位置的数据，返回下一个数据的位置。 

```cpp
	//删除区间内的元素

	//deqInt是用deque<int>声明的容器，现已包含按顺序的1,3,5,6,9元素。
	deque<int>::iterator itBegin=deqInt.begin()+1;
	deque<int>::iterator itEnd=deqInt.begin()+3;
	deqInt.erase(itBegin,itEnd);
	//此时容器deqInt包含按顺序的1,6,9三个元素。 

	//假设 deqInt 包含1,3,2,3,3,3,4,3,5,3，删除容器中等于3的元素
	for(deque<int>::iterator it=deqInt.being(); it!=deqInt.end(); )  {
        //小括号里不需写 ++it	
		if(*it == 3){
			it = deqInt.erase(it);    
            //以迭代器为参数，删除元素3，并把数据删除后的下一个元素位置返回给迭代器。
		    //此时，不执行 ++it； 
		}
		else{
		    ++it;
		}
	}
 
//删除deqInt的所有元素
deqInt.clear();         //容器为空
```

