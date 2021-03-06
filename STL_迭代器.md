# STL_迭代器

### 一、迭代器基本原理

迭代器是一个“可遍历STL容器内全部或部分元素”的对象。

迭代器指出容器中的一个特定位置。

迭代器就如同一个指针。

迭代器提供对一个容器中的对象的访问方法，并且可以定义了容器中对象的范围。

迭代器的类别：

- **输入迭代器**：也有叫法称之为“只读迭代器”，它从容器中读取元素，只能一次读入一个元素向前移动，只支持一遍算法，同一个输入迭代器不能两遍遍历一个序列。

- **输出迭代器**：也有叫法称之为“只写迭代器”，它往容器中写入元素，只能一次写入一个元素向前移动，只支持一遍算法，同一个输出迭代器不能两遍遍历一个序列。

- **正向迭代器**：组合输入迭代器和输出迭代器的功能，还可以多次解析一个迭代器指定的位置，可以对一个值进行多次读/写。

- **双向迭代器**：组合正向迭代器的功能，还可以通过--操作符向后移动位置。

- **随机访问迭代器**：组合双向迭代器的功能，还可以向前向后跳过任意个位置，可以直接访问容器中任何位置的元素。

目前本系列教程所用到的容器，都支持双向迭代器或随机访问迭代器，下面将会详细介绍这两个类别的迭代器。

### 二、双向迭代器与随机访问迭代器

双向迭代器支持的操作：it++, ++it,  it--,  --it，*it， itA = itB，itA == itB，itA != itB，其中list,set,multiset,map,multimap支持双向迭代器。

随机访问迭代器支持的操作：在双向迭代器的操作基础上添加it+=i， it-=i， it+i(或it=it+i)，it[i],itA<itB,  itA<=itB, itA>itB, itA>=itB 的功能。其中vector，deque支持随机访问迭代器。

### 三、vector与迭代器的配合使用                      

```cpp
vector<int> vecInt; //假设包含1,3,5,7,9元素

vector<int>::iterator it;      //声明容器vector<int>的迭代器。

it = vecInt.begin();  // *it == 1

++it;             //或者it++; *it == 3 ，前++的效率比后++的效率高，前++返回引用，后++返回值。

it += 2;       //*it == 7

it = it+1;      //*it == 9

++it;             // it == vecInt.end(); 此时不能再执行*it,会出错!

//正向遍历：
for(vector<int>::iterator it=vecInt.begin(); it!=vecInt.end(); ++it){
   	int iItem = *it; 
	cout << iItem;  //或直接使用 cout << *it;
}
//这样子便打印出1 3 5 7 9

//逆向遍历：
for(vector<int>::reverse_iterator rit=vecInt.rbegin(); rit!=vecInt.rend(); ++rit)  //注意，小括号内仍是++rit
{
   int iItem = *rit;
   cout << iItem;   //或直接使用cout << *rit;
}
//此时将打印出9,7,5,3,1
```

注意，这里迭代器的声明采用vector\<int>::reverse_iterator，而非vector\<int>::iterator。 

迭代器还有其它两种声明方法：

- vector\<int>::const_iterator 
- vector\<int>::const_reverse_iterator

 以上两种分别是vector\<int>::iterator 与vector\<int>::reverse_iterator 的只读形式，使用这两种迭代器时，不会修改到容器中的值。

备注：不过容器中的insert和erase方法仅接受这四种类型中的iterator，其它三种不支持。《Effective STL》建议我们尽量使用iterator取代const_iterator、reverse_iterator和const_reverse_iterator。 