# STL简介

### 一、基本概念

STL（Standard Template Library，标准模板库)是惠普实验室开发的一系列软件的统称。现然主要出现在C++中，但在被引入C++之前该技术就已经存在了很长的一段时间。

STL的从广义上讲分为三类：**algorithm（算法）、container（容器）和iterator（迭代器）**，容器和算法通过迭代器可以进行无缝地连接。几乎所有的代码都采 用了模板类和模板函数的方式，这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。

在C++标准中，STL 就位于各个 C++ 的头文件中，即它并非以二进制代码的形式提供，而是以源代码的形式提供。STL被组织为下面的13个头文 件：\<algorithm>、\<deque>、\<functional>、\<iterator>、\<vector>、\<list>、\<map>、\<memory>、\<numeric>、\<queue>、\<set>、\<stack> 和\<utility>。

STL详细的说六大组件：

1. **容器**：各种数据结构，如vector、list、deque、set、map等,用来存放数据，从实现角度来看，STL容器是一种class template。
2. **算法**：各种常用的算法，如sort、find、copy、for_each。从实现的角度来看，STL算法是一种function tempalte.
3. **迭代器**：扮演了容器与算法之间的胶合剂，共有五种类型，从实现角度来看，迭代器是一种将operator* , operator-> , operator++,operator–等指针相关操作予以重载的class template. 所有STL容器都附带有自己专属的迭代器，只有容器的设计者才知道如何遍历自己的元素。原生指针(native pointer)也是一种迭代器。
4. **仿函数**：行为类似函数，可作为算法的某种策略。从实现角度来看，仿函数是一种重载了operator()的class 或者class template
5. **适配器**：一种用来修饰容器或者仿函数或迭代器接口的东西。
6. **空间配置器**：负责空间的配置与管理。从实现角度看，配置器是一个实现了动态空间配置、空间管理、空间释放的class tempalte.

STL六大组件的交互关系，容器通过空间配置器取得数据存储空间，算法通过迭代器存储容器中的内容，仿函数可以协助算法完成不同的策略的变化，适配器可以修饰仿函数

### 二、STL优点

1）STL是C++的一部分，因此不用额外安装什么，它被内建在你的编译器之内。

2）STL的一个重要特点是数据结构和算法的分离。尽管这是个简单的概念，但是这种分离确实使得STL变得非常通用。

例如，在STL的vector容器中，可以放入元素、基础数据类型变量、元素的地址；

STL的sort()函数可以用来操作vector,list等容器。

3） 程序员可以不用思考STL具体的实现过程，只要能够熟练使用STL就OK了。这样他们就可以把精力放在程序开发的别的方面。

4） STL具有高可重用性，高性能，高移植性，跨平台的优点。

- 高可重用性：STL中几乎所有的代码都采用了模板类和模版函数的方式实现，这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。关于模板的知识，已经给大家介绍了。
- 高性能：如map可以高效地从十万条记录里面查找出指定的记录，因为map是采用红黑树的变体实现的。(红黑树是平横二叉树的一种)
- 高移植性：如在项目A上用STL编写的模块，可以直接移植到项目B上。
- 跨平台：如用windows的Visual Studio编写的代码可以在Mac OS的XCode上直接编译。

5） 程序员可以不用思考STL具体的实现过程，只要能够熟练使用STL就OK了。这样他们就可以把精力放在程序开发的别的方面。

6） 了解到STL的这些好处，我们知道STL无疑是最值得C++程序员骄傲的一部分。每一个C++程序员都应该好好学习STL。只有能够熟练使用STL的程序员，才是好的C++程序员。

7）  总之：招聘工作中，经常遇到C++程序员对STL不是非常了解。大多是有一个大致的映像，而对于在什么情况下应该使用哪个容器和算法都感到比较茫然。**STL是C++程序员的一项不可或缺的基本技能**，掌握它对提升C++编程大有裨益。

### 三、三大组件介绍

#### 容器

![image-20210122111838376](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20210122111838376.png)

几乎可以说，任何特定的数据结构都是为了实现某种特定的算法。STL容器就是将运用最广泛的一些数据结构实现出来。

常用的数据结构：**数组**(array) , **链表**(list), tree(**树**)，**栈**(stack), **队列**(queue), **集合**(set),**映射表**(map), 根据数据在容器中的排列特性，这些数据分为序列式容器和关联式容器两种。

- 序列式容器（Sequence containers）

  每个元素都有固定位置－－取决于插入时机和地点，和元素值无关。如：Vector容器、Deque容器、List容器等。

- 关联式容器（Associated containers）

  元素位置取决于特定的排序准则，和插入顺序无关。关联式容器是非线性的树结构，更准确的说是二叉树结构。各元素之间没有严格的物理上的顺序关系，也就是说元素在容器中并没有保存元素置入容器时的逻辑顺序。关联式容器另一个显著特点是：在值中选择一个值作为关键字key，这个关键字对值起到索引的作用，方便查找。Set/multiset容器 Map/multimap容器 

| 数据结构                 | 描述                                                         | 实现头文件 |
| ------------------------ | ------------------------------------------------------------ | ---------- |
| 向量(vector)             | 连续存储的元素                                               | \<vector>  |
| 列表(list)               | 由节点组成的双向链表，每个结点包含着一个元素                 | \<list>    |
| 双队列(deque)            | 连续存储的指向不同元素的指针所组成的数组                     | \<deque>   |
| 集合(set)                | 由节点组成的红黑树，每个节点都包含着一个元素，节点之间以某种作用于元素对的谓词排列，没有两个不同的元素能够拥有相同的次序 | \<set>     |
| 多重集合(multiset)       | 允许存在两个次序相等的元素的集合                             | \<set>     |
| 栈(stack)                | 后进先出的值的排列                                           | \<stack>   |
| 队列(queue)              | 先进先出的执的排列                                           | \<queue>   |
| 优先队列(priority_queue) | 元素的次序是由作用于所存储的值对上的某种谓词决定的的一种队列 | \<queue>   |
| 映射(map)                | 由{键，值}对组成的集合，以某种作用于键对上的谓词排列         | \<map>     |
| 多重映射(multimap)       | 允许键对有相等的次序的映射                                   | \<map>     |

#### 迭代器

迭代器从作用上来说是最基本的部分，可是理解起来比前两者都要费力一些。软件设计有一个基本原则，所有的问题都可以通过引进一个间接层来简化， 这种简化在STL中就是用迭代器来完成的。概括来说，迭代器在STL中用来将算法和容器联系起来，起着一种黏和剂的作用。几乎STL提供的所有算法都是通 过迭代器存取元素序列进行工作的，每一个容器都定义了其本身所专有的迭代器，用以存取容器中的元素。

迭代器部分主要由头文件\<utility>,\<iterator>和\<memory>组 成。\<utility>是一个很小的头文件，它包括了贯穿使用在STL中的几个模板的声明，\<iterator>中提供了迭代器 使用的许多方法，而对于\<memory>的描述则十分的困难，它以不同寻常的方式为容器中的元素分配存储空间，同时也为某些算法执行期间产生 的临时对象提供机制,\<memory>中的主要部分是模板类allocator，它负责产生所有容器中的默认分配器。

迭代器的种类:

| 迭代器         | 功能                                                         | 描述                                   |
| -------------- | ------------------------------------------------------------ | -------------------------------------- |
| 输入迭代器     | 提供对数据的只读访问                                         | 只读，支持++、==、！=                  |
| 输出迭代器     | 提供对数据的只写访问                                         | 只写，支持++                           |
| 前向迭代器     | 提供读写操作，并能向前推进迭代器                             | 读写，支持++、==、！=                  |
| 双向迭代器     | 提供读写操作，并能向前和向后操作                             | 读写，支持++、–，                      |
| 随机访问迭代器 | 提供读写操作，并能以跳跃的方式访问容器的任意数据，是功能最强的迭代器 | 读写，支持++、–、[n]、-n、<、<=、>、>= |

#### 算法

函数库对数据类型的选择对其可重用性起着至关重要的作用。举例来说，一个求方根的函数，在使用浮点数作为其参数类型的情况下的可重用性肯定比使 用整型作为它的参数类性要高。而C++通过模板的机制允许推迟对某些类型的选择，直到真正想使用模板或者说对模板进行特化的时候，STL就利用了这一点提 供了相当多的有用算法。它是在一个有效的框架中完成这些算法的——可以将所有的类型划分为少数的几类，然后就可以在模版的参数中使用一种类型替换掉同一种 类中的其他类型。

STL提供了大约100个实现算法的模版函数，比如算法for_each将为指定序列中的每一个元素调用指定的函数，stable_sort以你所指定的规则对序列进行稳定性排序等等。这样一来，只要熟悉了STL之后，许多代码可以被大大的化简，只需要通过调用一两个算法模板，就可以完成所需要 的功能并大大地提升效率。

算法部分主要由头文件\<algorithm>，\<numeric>和\<functional>组 成。\<algorithm>是所有STL头文件中最大的一个（尽管它很好理解），它是由一大堆模版函数组成的，可以认为每个函数在很大程度上 都是独立的，其中常用到的功能范围涉及到比较、交换、查找、遍历操作、复制、修改、移除、反转、排序、合并等等。\<numeric>体积很 小，只包括几个在序列上面进行简单数学运算的模板函数，包括加法和乘法在序列上的一些操作。\<functional>中则定义了一些模板类， 用以声明函数对象。

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//STL 中的容器 算法 迭代器
void test01(){
	vector<int> v; //STL 中的标准容器之一 ：动态数组
	v.push_back(1); //vector 容器提供的插入数据的方法
	v.push_back(5);
	v.push_back(3);
	v.push_back(7);
	//迭代器
	vector<int>::iterator pStart = v.begin(); //vector 容器提供了 begin()方法 返回指向第一个元素的迭代器
	vector<int>::iterator pEnd = v.end(); //vector 容器提供了 end()方法 返回指向最后一个元素下一个位置的迭代器
	//通过迭代器遍历
	while (pStart != pEnd){
		cout << *pStart << " ";
		pStart++;
	}
	cout << endl;
	//算法 count 算法 用于统计元素的个数
	int n = count(pStart, pEnd, 5);
	cout << "n:" << n << endl;
}
//STL 容器不单单可以存储基础数据类型，也可以存储类对象
class Teacher
{
public:
	Teacher(int age) :age(age){};
	~Teacher(){};
public:
	int age;
};
void test02(){
	vector<Teacher> v; //存储 Teacher 类型数据的容器
	Teacher t1(10), t2(20), t3(30);
	v.push_back(t1);
	v.push_back(t2);
	v.push_back(t3);
	vector<Teacher>::iterator pStart = v.begin();
	vector<Teacher>::iterator pEnd = v.end();
	//通过迭代器遍历
	while (pStart != pEnd){
		cout << pStart->age << " ";
		pStart++;
	}
	cout << endl;
}
//存储 Teacher 类型指针
void test03(){
	vector<Teacher*> v; //存储 Teacher 类型指针
	Teacher* t1 = new Teacher(10);
	Teacher* t2 = new Teacher(20);
	Teacher* t3 = new Teacher(30);
	v.push_back(t1);
	v.push_back(t2);
	v.push_back(t3);
	//拿到容器迭代器
	vector<Teacher*>::iterator pStart = v.begin();
	vector<Teacher*>::iterator pEnd = v.end();
	//通过迭代器遍历
	while (pStart != pEnd){
		cout << (*pStart)->age << " ";
		pStart++;
	}
	cout << endl;
}
//容器嵌套容器 难点
void test04()
{
	vector< vector<int> > v;
	vector<int>v1;
	vector<int>v2;
	vector<int>v3;

	for (int i = 0; i < 5;i++)
	{
		v1.push_back(i);
		v2.push_back(i * 10);
		v3.push_back(i * 100);
	}
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);

	for (vector< vector<int> >::iterator it = v.begin(); it != v.end();it++)
	{
		for (vector<int>::iterator subIt = (*it).begin(); subIt != (*it).end(); subIt ++)
		{
			cout << *subIt << " ";
		}
		cout << endl;
	}
} 
int main(){
	//test01();
	//test02();
	//test03();
	test04();
	system("pause");
	return 0;
}
```

部分内容来自：

[【C/C++】STL详解]: https://blog.csdn.net/qq_42322103/article/details/99685797

