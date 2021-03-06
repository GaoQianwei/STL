# STL_综合案例

学校演讲比赛
1）某市举行一场演讲比赛，共有24个人参加，按参加顺序设置参赛号。比赛共三轮，前两轮为淘汰赛，第三轮为决赛。
2）比赛方式：分组比赛
	第一轮分为4个小组，根据参赛号顺序依次划分，比如100-105为一组，106-111为第二组，依次类推，每组6个人，每人分别按参赛号顺序演讲。
当小组演讲完后，淘汰组内排名最后的三个选手，然后继续下一个小组的比赛。
	第二轮分为2个小组，每组6人，每个人分别按参赛号顺序演讲。当小组完后，淘汰组内排名最后的三个选手，然后继续下一个小组的比赛。
	第三轮只剩下6个人，本轮为赛决，选出前三名。
	选手每次要随机分组，进行比赛。
3）比赛评分：10个评委打分，去除最低、最高分，求平均分
	每个选手演讲完由10个评委分别打分。该选手的最终得分是去掉一个最高分和一个最低分，求得剩下的8个成绩的平均分。选手的名次按得分降序排列，
若得分一样，按参赛号升序排名。

用STL编程，求解一下问题

1. 请打印出所有选手的名字与参赛号，并以参赛号的升序排列。
2. 打印每一轮比赛前，分组情况
3. 打印每一轮比赛后，小组晋级名单
4. 打印决赛前三名，选手名称、成绩。

### main.cpp文件

```cpp
#include "manager.h"
using namespace std;
int main(void)
{
	Manager m;
	m.show();
	return 0;
}
```

### Speaker.h文件

```cpp
#pragma once
#include <string>
#include <vector>
#include <iostream>
using namespace std;
class Speaker
{
public:
	void setName(string s);
	string getName();
	void setScore(int n);
	int getScore(int n);
	void setNumber(int n);
	int getNumber();
private:
	string name;
	int number;
	vector<int>scores;
};
```



### Speaker.cpp文件

```cpp
#include "Speaker.h"

void Speaker::setName(string s) {
	this->name = s;
}
string Speaker::getName() {
	return this->name;
}
void Speaker::setScore(int n) {//n是第n+1次成绩
	this->scores.push_back(n);
}
int Speaker::getScore(int n) {
	if (n < 0 || n>2)
		cout << "error";
	return this->scores[n];
}

void Speaker::setNumber(int n) {
	this->number = n;
}

int Speaker::getNumber() {
	return this->number;
}
```



### Manager.h文件

```cpp
#pragma once
#define SPEAKER_NUMBER 24
#include <string>
#include <vector>
#include <map>
#include <iostream>
#include <algorithm>
#include <time.h>
#include <deque>
#include <functional>
#include "Speaker.h"
using namespace std;
class Manager
{
public:
	void create_speaker();
	void divid_speaker();
	void first_game();
	void second_game();
	void final_game();
	void show();
private:
	vector<Speaker> first_vector;
	vector<Speaker> second_vector;
	vector<Speaker> third_vector;
	multimap<int,Speaker> first_groups;
	multimap<int, Speaker> second_groups;
	multimap<int, Speaker> third_groups;
};

```



### Manager.cpp文件

```cpp
#include "Manager.h"

int get_avg_score() {
	deque<int>score_list;
	for (int i = 0; i < 10; i++) {
		int score = rand() % 61 + 40;
		score_list.push_back(score);
	}
	sort(score_list.begin(), score_list.end());
	score_list.pop_back();
	score_list.pop_front();
	int total_score = 0;
	for (deque<int>::iterator it = score_list.begin(); it != score_list.end(); it++) {
		total_score += (*it);
	}
	return total_score / score_list.size();
}

void print_info_after_game(multimap<int, Speaker>::iterator p,int n) {
	cout << "姓名:" << (*p).second.getName()
		<< "   编号：" << (*p).second.getNumber()
		<< "   组号：" << (*p).first
		<< "   分数：" << (*p).second.getScore(n)
		<< endl;
}

void divide_and_print(vector<Speaker> &v, multimap<int, Speaker> &m) {
	int flag = 1;
	for (vector<Speaker>::iterator it = v.begin(); it != v.end(); ) {
		for (int j = 0; j < 6; j++, it++) {
			m.insert(make_pair(flag, *it));
		}
		flag++;
	}
	cout << "第二次分组：" << endl;
	for (multimap<int, Speaker>::iterator p = m.begin(); p != m.end(); p++) {
		cout << "姓名:" << (*p).second.getName() << "   编号：" << (*p).second.getNumber() << "   组号：" << (*p).first << endl;
	}
}

void print_info_after_game(multimap<int, Speaker> &m,vector<Speaker> &v) {	
	for (multimap<int, Speaker>::iterator p = m.begin(); p != m.end(); p++) {
		print_info_after_game(p, 0);
		v.push_back((*p).second);
	}
}

void Manager::create_speaker() {
	string name_seed = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
	for (int i = 0; i < SPEAKER_NUMBER; i++) {

		Speaker speaker;
		string tmp = name_seed.substr(i, 1);
		speaker.setName("选手" + tmp);
		this->first_vector.push_back(speaker);
	}
}

bool compare(Speaker &s1,Speaker &s2) {
	return s1.getNumber() < s2.getNumber();
}

void Manager::divid_speaker() {
	//随机分配编号
	random_shuffle(this->first_vector.begin(), this->first_vector.end()); 
	int ij = 0;
	for (vector<Speaker>::iterator it = this->first_vector.begin(); it != this->first_vector.end(); ij++,it++) {
		this->first_vector[ij].setNumber(ij+100);
	}
	sort(this->first_vector.begin(),this->first_vector.end(),compare);
	divide_and_print(this->first_vector, this->first_groups);	
}

bool compare_score(Speaker& s1, Speaker& s2) {
	return s1.getScore(0) > s2.getScore(0);
}

void Manager::first_game() {
	srand(time(nullptr));
	for (multimap<int, Speaker>::iterator p = this->first_groups.begin(); p != this->first_groups.end(); p++) {
		//cout << "姓名:" << (*p).second.getName() << "   编号：" << (*p).second.getNumber() << "   组号：" << (*p).first << endl;
		int avg_score = get_avg_score();
		(*p).second.setScore(avg_score);
	}
	cout << "第一次比赛后：" << endl;
	print_info_after_game(this->first_groups, this->second_vector);
		
	//先按分数排序，再去掉后一半，再按编号排序
	sort(this->second_vector.begin(),this->second_vector.end(),compare_score);
	this->second_vector.erase(this->second_vector.begin()+12,this->second_vector.end());
	sort(this->second_vector.begin(), this->second_vector.end(), compare);
	cout << "第一次比赛后晋级名单:" << endl;
	for (vector<Speaker>::iterator it = this->second_vector.begin(); it != this->second_vector.end(); it++) {
		cout << (*it).getName() << "  "<<(*it).getNumber()<<endl;
	}
	divide_and_print(this->second_vector, this->second_groups);
}

bool compare_score_second(Speaker& s1, Speaker& s2) {
	return s1.getScore(1) > s2.getScore(1);
}

void Manager::second_game() {
	srand(time(nullptr));
	for (multimap<int, Speaker>::iterator p = this->second_groups.begin(); p != this->second_groups.end(); p++) {
		//cout << "姓名:" << (*p).second.getName() << "   编号：" << (*p).second.getNumber() << "   组号：" << (*p).first << endl;
		int avg_score = get_avg_score();
		(*p).second.setScore(avg_score);
	}
	cout << "第二次比赛后：" << endl;
	print_info_after_game(this->second_groups, this->third_vector);

	//先按分数排序，再去掉后一半，再按编号排序
	sort(this->third_vector.begin(), this->third_vector.end(), compare_score_second);
	this->third_vector.erase(this->third_vector.begin() + 6, this->third_vector.end());
	sort(this->third_vector.begin(), this->third_vector.end(), compare);
	cout << "第二次比赛后晋级名单:" << endl;
	for (vector<Speaker>::iterator it = this->third_vector.begin(); it != this->third_vector.end(); it++) {
		cout << (*it).getName() << "  " << (*it).getNumber() << endl;
	}	
}

bool compare_score_third(Speaker& s1, Speaker& s2) {
	return s1.getScore(2) > s2.getScore(2);
}

void Manager::final_game() {
	srand(time(nullptr));
	for (vector<Speaker>::iterator p = this->third_vector.begin(); p != this->third_vector.end(); p++) {
		int avg_score = get_avg_score();
		(*p).setScore(avg_score);
	}

	cout << "第三次比赛后：" << endl;
	for (vector<Speaker>::iterator p = this->third_vector.begin(); p != this->third_vector.end(); p++) {
		cout << "姓名:" << (*p).getName()<< "   编号：" << (*p).getNumber()<< "   分数：" << (*p).getScore(2)	<< endl;
	}

	//先按分数排序，再去掉后一半，再按编号排序
	sort(this->third_vector.begin(), this->third_vector.end(), compare_score_third);
	this->third_vector.erase(this->third_vector.begin() + 3, this->third_vector.end());
	cout << "前三名名单:" << endl;
	for (vector<Speaker>::iterator it = this->third_vector.begin(); it != this->third_vector.end(); it++) {
		cout << "姓名:" << (*it).getName() << "   编号："<< (*it).getNumber() << "   分数：" <<(*it).getScore(2)<< endl;
	}
}

void Manager::show() {
	this->create_speaker();
	this->divid_speaker();
	this->first_game();
	this->second_game();
	this->final_game();
}
```

