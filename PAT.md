# PAT刷题（甲级）
## 1003 Emergency
迪杰斯特拉(Dijkstra)算法

```c++
#include<stdio.h>
#include<iostream>
#include<vector>

using namespace std;

int main() {
	int N;//city num
	int M;//road num
	int currentCity;
	int saveCity;
	int city1;
	int city2;
	int dist;
	cin >> N >> M >> currentCity >> saveCity;
	vector<int> teams(N, 0);
	vector<vector<int>> distances(N, vector<int>(N, 9999));
	for (int i = 0; i < N; i++) {
		cin >> teams[i];
	}
	for (int i = 0; i < M; i++) {
		cin >> city1 >> city2 >> dist;
		distances[city1][city2] = dist;
		distances[city2][city1] = dist;
	}
	
	/*for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			cout << distances[i][j] << " ";
		}
		cout << endl;
	}*/

	vector<int> visited(N, 0);
	vector<int> shortestPath(N, 9999);
	vector<int> maxpeople(N, 0);
	vector<int> paths(N, 0);
	int selected;
	selected = currentCity;
	shortestPath[currentCity] = 0;
	maxpeople[currentCity] = teams[currentCity];
	paths[currentCity] = 1;
	for (int i = 0; i < N; i++) {
		visited[selected] = 1;
		for (int j = 0; j < N; j++) {
			if (visited[j] != 1 && distances[selected][j] != 9999 && shortestPath[j] > shortestPath[selected] + distances[selected][j]) {
				//更短路径
				shortestPath[j] = shortestPath[selected] + distances[selected][j];
				maxpeople[j] = maxpeople[selected] + teams[j];
				paths[j] = paths[selected];
			}
			else if(visited[j] != 1 && distances[selected][j] != 9999 && shortestPath[j] == shortestPath[selected] + distances[selected][j]){
				//同样短的路径
				if (maxpeople[selected] + teams[j] > maxpeople[j]) {
					maxpeople[j] = maxpeople[selected] + teams[j];
				}
				paths[j] += paths[selected];
			}
		}
		int min = 9999;
		int minindex = 0;
		for (int j = 0; j < N; j++) {
			if (visited[j] != 1 && shortestPath[j] < min) {
				min = shortestPath[j];
				minindex = j;
			}
		}
		
		selected = minindex;
		//cout << selected << endl;
		//for (int j = 0; j < N; j++) {
		//	cout << shortestPath[j] << " ";
		//	//cout << visited[j] << " ";
		//}
		//cout << endl;
	}

	cout << paths[saveCity] << " " << maxpeople[saveCity];
}
```
## 1004 Counting Leaves
整体思路，先全部读入，再进行层序遍历。  
因为数据读入的顺序可能不是有序的。  
例如：1->2->3  
如果输入输入数据是  
2 1 3  
1 1 2  
则无法直接去更新2结点的level

最后需要注意的测试点是N==1的特殊情况（只有根节点）

```c++
#include<stdio.h>
#include<iostream>
#include<queue>

using namespace std;

class Node {
public:
	int isLeave;
	int level;
	int parent;
	int id;
	Node() {
		this->level = -1;
		this->isLeave = 1;
		this->parent = -1;
	}
};

int main() {
	int num, nun_leave_num;
	cin >> num >> nun_leave_num;
	if (num == 1) {
		cout << 1;
		return 0;
	}
	int thisnode;
	int thischild;
	int childnum;
	Node nodes[101];
	for (int i = 0; i <= 100; i++) {
		nodes[i].id = i;
	}
	queue<Node> nodeQueue;
	for (int i = 0; i < nun_leave_num; i++) {
		cin >> thisnode >> childnum;
		nodes[thisnode].isLeave = 0;
		for (int j = 0; j < childnum; j++) {
			cin >> thischild;
			nodes[thischild].parent = thisnode;
		}
	}
	nodes[1].level = 1;
	nodeQueue.push(nodes[1]);
	int maxlevel = 0;
	while (!nodeQueue.empty()) {
		for (int i = 1; i <= 100; i++) {
			if (nodes[i].parent == nodeQueue.front().id) {
				nodes[nodeQueue.front().id].isLeave = 0;
				nodeQueue.push(nodes[i]);
			}
		}
		if (nodeQueue.front().id != 1) {
			nodes[nodeQueue.front().id].level = nodes[nodeQueue.front().parent].level + 1;
			if (maxlevel < nodes[nodeQueue.front().id].level) {
				maxlevel = nodes[nodeQueue.front().id].level;
			}
		}
		nodeQueue.pop();
	}
	for (int i = 1; i <= maxlevel; i++) {
		int num = 0;
		for (int j = 1; j <= 100; j++) {
			if (nodes[j].isLeave == 1 && nodes[j].level == i) {
				num++;
			}
		}
		cout << num;
		if (i != maxlevel) {
			cout << " ";
		}
	}
	//cout << nodes[2].parent << nodes[2].level << nodes[2].isLeave;
}
```

## 1005 Spell It Right
用字符串每个字符求和，然后将和转为字符串，根据新字符串的每个字符输出对应英文。

```c++
#include<stdio.h>
#include<iostream>
#include<map>

using namespace std;

int main() {
	string s1, s2;
	cin >> s1;
	int sum = 0;
	map<char, string> dict;
	dict['0'] = "zero";
	dict['1'] = "one";
	dict['2'] = "two";
	dict['3'] = "three";
	dict['4'] = "four";
	dict['5'] = "five";
	dict['6'] = "six";
	dict['7'] = "seven";
	dict['8'] = "eight";
	dict['9'] = "nine";
	for (int i = 0; i < s1.length(); i++) {
		sum += s1[i] - '0';
	}
	//cout << sum;
	if (sum == 0) {
		cout << "zero";
		return 0;
	}
	while (sum != 0) {
		s2 += '0' + sum % 10;
		sum /= 10;
	}
	//cout << s2;
	for (int i = s2.length() - 1; i >= 0; i--) {
		cout << dict[s2[i]];
		if (i != 0) {
			cout << " ";
		}
	}
}
```

## 1006 Sign In and Sign Out 
题目本身并不难，只要读入类/结构体就行。  
遇到的困难在于，开始的时候使用了一个vector来存储类的对象的指针
```c++
vector<Record*> records(N, new Record);
```
我以为可以通过这句话来生成N个指向不同对象的指针，然后存入vector，实际上好像只是生成了N个不同指针，但是指向的都是同一个物体。  
因此最后还是老老实实地每次生成一个新的对象然后push到vector里。
```c++
#include<stdio.h>
#include<iostream>
#include<vector>

using namespace std;

class Record {
public:
	string name;
	int in_time;
	int out_time;
	Record(string name) {
		this->name = name;
		in_time = -1;
		out_time = -1;
	}
};

int main() {
	int N;
	cin >> N;
	vector<Record> records;

	int hour1, hour2, minute1, minute2, second1, second2;
	int maxTime = 0;
	int minTime = 90000;
	int maxindex = -1;
	int minindex = -1;
	string name;
	for (int i = 0; i < N; i++) {
		//cin >> records[i]->name;
		cin >> name;
		Record newRecord(name);
		records.push_back(newRecord);
		scanf(" %d:%d:%d %d:%d:%d", &hour1, &minute1, &second1, &hour2, &minute2, &second2);
		records[i].in_time = hour1 * 3600 + minute1 * 60 + second1;
		records[i].out_time = hour2 * 3600 + minute2 * 60 + second2;
		if (records[i].in_time < minTime) {
			minTime = records[i].in_time;
			minindex = i;
		}
		if (records[i].out_time > maxTime) {
			maxTime = records[i].out_time;
			maxindex = i;
		}
		//cout << minindex << " " << maxindex << endl;
	}
	//cout << minindex << " " << maxindex << endl;
	//cout << records[0].in_time << " " << records[1].in_time << " " << records[2].in_time;
	cout << records[minindex].name << " " << records[maxindex].name;
}
```

## 1007 Maximum Subsequence Sum
经典动态规划题目，求最大子序列和。

首先用DP算法去计算以各个位子结尾的数组的最大值。  
状态转移方程  
```c++
if(dp[i-1] <= 0){
	dp[i] = nums[i];
}
else{
	dp[i] = dp[i-1] + nums[i];
}
```
然后遍历bp数组去获得最大和的大小和终止坐标index2，然后从index2的位置向前遍历，当和result为最大和max_sum的时候，更新index1。  
需要注意的是，第一次写的时候在找到result == max_sum的时候就break了。后来发现这样是不对的，因为要找到最小的index1,在第一个找到的index1之前，可能若干个数字的和为0，那么就可以获得更小的index1。
```c++
#include<stdio.h>
#include<iostream>
#include<vector>
using namespace std;

int main() {
	int N;
	cin >> N;
	vector<int> nums(N, 0);
	vector<int> max_sum(N, 0);
	for (int i = 0; i < N; i++) {
		cin >> nums[i];
	}
	for (int i = 0; i < N; i++) {
		if (i > 0 && max_sum[i - 1] > 0) {
			max_sum[i] = max_sum[i - 1] + nums[i];
		}
		else {
			max_sum[i] = nums[i];
		}
	}
	int sum = nums[0];
	int index1 = 0;
	int index2 = 0;
	
	for (int i = 0; i < N; i++) {
		if (max_sum[i] > sum) {
			sum = max_sum[i];
			index2 = i;
		}
	}
	if (sum < 0) {
		cout << "0 " << nums[0] << " " << nums[N - 1];
		return 0;
	}
	int result = 0;
	for (int i = index2; i >= 0; i--) {
		result += nums[i];
		if (result == sum) {
			index1 = i;
		}
	}
	cout << sum << " " << nums[index1] << " " << nums[index2];
}
```
## 1008 Elevator
就遍历一遍就好了...没什么好说的
```c++
#include<stdio.h>
#include<iostream>
#include<vector>
using namespace std;

int main() {
	int N;
	cin >> N;
	vector<int> nums(N, 0);
	int sum = 0;
	for (int i = 0; i < N; i++) {
		cin >> nums[i];
		if (i == 0) {
			sum += 6 * nums[0] + 5;
		}
		else if (nums[i] > nums[i - 1]) {
			sum += 6 * (nums[i] - nums[i - 1]) + 5;
		}
		else {
			sum += 4 * (nums[i - 1] - nums[i]) + 5;
		}
	}
	cout << sum;
}
```
## 1009 Product of Polynomials
定义一个类来存储每个结点的指数和系数。  
然后，把第一个多项式存入一个vector，第二个多项式存入第二个vector。然后相乘之后输出系数不为0的项。

需要注意的是，我在写的时候贪图省力，开始用了cout，后来因为题目需要保留一位小数，我就使用了printf。我记得WK讲过两者不可混用，但是测试点都过了。

好像是cin/cout是有缓冲区的，printf是没有的。
```c++
#include<stdio.h>
#include<iostream>
#include<vector>
using namespace std;

class Node {
public:
	double exponents;
	double coefficients;
	Node() {
		exponents = -1;
		coefficients = -1;
	}
	Node(double e, double c) {
		this->exponents = e;
		this->coefficients = c;
	}
};

int main() {
	
	vector<Node> list1;
	vector<Node> list2;
	int N1;
	cin >> N1;
	double exp, coe;
	int length1, length2;
	for (int i = 0; i < N1; i++) {
		cin >> exp >> coe;
		Node newNode(exp, coe);
		list1.push_back(newNode);
		if (i == 0) {
			length1 = exp;
		}
	}
	int N2;
	cin >> N2;
	for (int i = 0; i < N2; i++) {
		cin >> exp >> coe;
		//cout << exp << " " << coe << endl;
		Node newNode(exp, coe);
		list2.push_back(newNode);
		if (i == 0) {
			length2 = exp;
		}
	}
	//cout << length1 + length2;
	vector<double> result(length1 + length2 + 1, 0);
	int num = 0;
	for (int i = 0; i < list1.size(); i++) {
		for (int j = 0; j < list2.size(); j++) {
			if (result[list1[i].exponents + list2[j].exponents] == 0) {
				num++;
				//cout << " 1";

			}
			else if(result[list1[i].exponents + list2[j].exponents] + list1[i].coefficients * list2[j].coefficients == 0){
				num--;
			}
			result[list1[i].exponents + list2[j].exponents] += list1[i].coefficients * list2[j].coefficients;
			//cout << result[2] << " ";
		}
	}
	//cout << list1[1].coefficients;
	cout << num;
	for (int i = result.size() - 1; i >= 0; i--) {
		if (result[i] != 0) {
			printf(" %d %.1f", i, result[i]);
			//cout << " " << result[i];
		}
	}
}
```
## 1010 Radix
题目本身思路清晰，先把确定的数字转为十进制，然后去试另一个数字。  
一开始使用的是暴力解法，从2进制开始，一直往上，直到转换的数字大于确定数字的十进制。但是这样的作法会超时，因此需要用二分法。  
二分法上界为确定的数的十进制+1，下界为不确定数字中最大数字+1。  
还有比较坑的点在于，因为进制很大会造成溢出，不能用int,应该用long long，但是long long也可能溢出；溢出后变为负数，所以判断时加上一句
```c++
convert(num2, middle) > result1 || convert(num2, middle) < 0
```
```c++
#include<stdio.h>
#include<iostream>
#include<vector>
using namespace std;

int getNum(char ch) {
	if (ch >= '0' && ch <= '9') {
		return ch - '0';
	}
	else if (ch >= 'a' && ch <= 'z') {
		return ch - 'a' + 10;
	}
	else {
		return -1;
	}
}

long long convert(string num, int radix) {
	long long sum = 0;
	long long multiply = 1;
	for (int i = num.length() - 1; i >= 0; i--) {
		sum += (long long)(getNum(num[i])) * multiply;
		multiply *= radix;
		//cout << sum << endl;
	}
	return sum;
}

int findMin(string num) {
	int max = 0;
	for (int i = 0; i < num.size(); i++) {
		if (max < getNum(num[i])) {
			max = getNum(num[i]);
		}
	}
	return max;
}

int main() {
	string num1, num2;
	int tag, radix;
	cin >> num1 >> num2 >> tag >> radix;
	//cout << num1 << num2 << tag << radix;
	long long result1, result2;
	if (tag == 1) {
		result1 = convert(num1, radix);
	}
	else {
		result1 = convert(num2, radix);
		num2 = num1;
	}
	//int myRadix = 2;
	//cout << convert("110", 2);
	/*while (1) {
		result2 = convert(num2, myRadix);
		if (result1 == result2) {
			cout << myRadix;
			return 0;
		}
		else if (result2 > result1) {
			cout << "Impossible";
			return 0;
		}
		myRadix++;
	}*/
	long long left = findMin(num2) + 1;
	long long right = result1 + 1;
	long long middle;
	while (left <= right) {
		middle = (left + right) / 2;
		if (convert(num2, middle) == result1) {
			cout << middle;
			return 0;
		}
		else if (convert(num2, middle) > result1 || convert(num2, middle) < 0) {
			right = middle - 1;
		}
		else {
			left = middle + 1;
		}
	}
	cout << "Impossible";
}
```