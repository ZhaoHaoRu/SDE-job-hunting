# STL



## priority_queue

### `priority_queue` 的构造

- **less 表示数字大的优先级高，而 greater 表示数字小的优先级高**

```cpp
//下面两种优先队列的定义是等价的
priority_queue<int> q;
priority_queue<int,vector<int>,less<int> >;//后面有一个空格
```

- 自定义比较函数
    - 结构体内部重载

```cpp
struct student{
        int grade;
        string name;
 
        //重载运算符，grade 值高的优先级大
        friend operator < (student s1,student s2){
            return s1.grade < s2.grade;
        }
    };
void test2(){
        priority_queue<student> q;
        student s1,s2,s3;
        s1.grade = 90;
        s1.name = "Tom";
 
        s2.grade = 80;
        s2.name = "Jerry";
 
        s3.grade = 100;
        s3.name = "Kevin";
 
        q.push(s1);
        q.push(s2);
        q.push(s3);
 
        while(!q.empty()){
            cout<<q.top().name<<":"<<q.top().grade<<endl;
            q.pop();
        } 
 
    }
        /*
         结果：
            Kevin:100
            Tom:90
            Jerry:80
        */
```

- 把重载的函数写在结构体外面

```cpp
struct fruit{
        string name;
        int price;
};
struct cmp{
    // "<" 表示 price 大的优先级高
    bool operator() (fruit f1,fruit f2){
        return f1.price < f2.price;
    }
};
void test3(){
        priority_queue<fruit,vector<fruit>,cmp> q;
        fruit f1,f2,f3;
 
        f1.name = "apple";
        f1.price = 5;
 
        f2.name = "banana";
        f2.price = 6;
 
        f3.name = "pear";
        f3.price = 7;
 
        q.push(f1);
        q.push(f2);
        q.push(f3);
 
        while(!q.empty()){
            cout<<q.top().name<<":"<<q.top().price<<endl;
            q.pop();
        }
  }
 
    /*
     结果：
           pear:7
           banana:6
           apple:5
    */

```

- 自定义元素比较

```cpp
auto my_comp = [](pair<string, int>& p1, pair<string, int>& p2) {
            return p1.second > p2.second || (p1.first < p2.first && p1.second == p2.second);
};
priority_queue<pair<string, int>, vector<pair<string, int>>, decltype(my_comp)> heap{my_comp};
```

### priority_queue的常用操作

- **元素访问**
    - top()：top()可以获得队首元素(即堆顶元素)，时间复杂度为O(1)
- **容量**
    - empty()：时间复杂度为O(1)
    - size()：时间复杂度为O(1)
- 修改器
    - pop() ：pop()令队首元素(即堆顶元素)出队，时间复杂度为O(logN),其中N为当前优先队列中的元素个数
    - push()：push(x)将令x入队，时间复杂度为O(logN),其中N为当前优先队列中的元素个数。



## String

> 注意string并没有`emplace_back`的用法！！！
> 

### 将数值类型转换为string

```cpp
string to_string (int val);
string to_string (long val);
string to_string (long long val);
string to_string (unsigned val);
string to_string (unsigned long val);
string to_string (unsigned long long val);
string to_string (float val);
string to_string (double val);
string to_string (long double val);
```

## List

### list 的遍历

```cpp
	auto itor = myList.begin();
	while(itor!=myList.end())
    {
        cout<< *itor++<<endl;
    }
```

### list 在遍历时删除元素

```cpp
for(list<st_user*>::iterator iter = list1.begin(); iter != list1.end();)
	{
		if( (*t)->id == 2 )
		{
			iter = list1.erase(t);
		}
		else
			++iter;
 
	}

```



## vector

### 二维数组的初始化

```cpp
vector<vector<bool>> visited(m, vector<bool>(n, false));
```

### assign: 替换容器中的内容

- **基本使用方法**

```cpp
void assign( size_type count, const T& value );(C++20 前)
constexpr void assign( size_type count, const T& value );(C++20 起)

template< class InputIt >
void assign( InputIt first, InputIt last );(C++20 前)
template< class InputIt >
constexpr void assign( InputIt first, InputIt last );(C++20 起)

void assign( std::initializer_list<T> ilist );(C++11 起)(C++20 前)
constexpr void assign( std::initializer_list<T> ilist );
(C++20 起)

```

> 说明
> 

1) 以 `count` 份 `value` 的副本替换内容。

2) 以范围`[first, last)`中元素的副本替换内容。若任一参数是指向 `*this`  中的迭代器则行为未定义。

3) 以来自 initializer_list `ilist` 的元素替换内容。

- **时间复杂度**

1) 与 `count` 成线性

2) 与 `first` 和 `last` 间的距离成线性

3) 与 ilist.size() 成线性

## map

### **lower_bound (set也有相同的用法）**

**指向首个*不小于* `key`的元素的迭代器。若找不到这种元素，则返回尾后迭代器（见 [end()](https://qingcms.gitee.io/cppreference/20210212/zh/cpp/container/map/end.html)）**

- 基本使用方法

```cpp
iterator lower_bound( const Key& key );(1)	
const_iterator lower_bound( const Key& key ) const;(1)	
template< class K > iterator lower_bound(const K& x);(2)	(C++14 起)
template< class K > const_iterator lower_bound(const K& x) const;(2)	(C++14 起)
```

### 求和

```cpp
#include <numeric>
accumulate(v.begin(), v.end(), 0);
```

### 逆序排序

```cpp
std::array<int, 10> s = {5, 7, 4, 2, 8, 6, 1, 9, 0, 3};
std::sort(s.begin(), s.end(), std::greater<int>());
```

### 遍历（直接获取键和值）

```cpp
int g = 0;
for (auto [k, v]: dict) {
    g = gcd(g, v);
}
```



## String和vector的相互转换

- vector → string

```cpp
std::string Str;

std::vector<uint8_t> Vec(6, 7);

Str.assign(Vec.begin(), Vec.end());
```

- string → vector

```cpp
std::string Str = "hello world!";

std::vector<uint8_t> Vec;

Vec.assign(Str.begin(), Str.end());
```



## KV容器

### map遍历

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;
int main() { 

unordered_map<string, int> mp;
mp["张三"] = 20;
mp["李四"] = 18;
mp["王五"] = 30;
// 方式一、迭代器
cout << "方式一、迭代器" << endl;
for (auto it = mp.begin(); it != mp.end(); it++) { 
	cout << it -> first << " " << it -> second << endl;
}
// 方式二、range for C++ 11版本及以上
cout << "\n方法二、 range for" << endl;
for (auto it : mp) { 
	cout << it.first << " " << it.second << endl;
}
// 方法三、 C++ 17版本及以上
cout << "\n方法三" << endl;
for (auto [key, val] : mp) { 
	cout << key  << " " << val << endl;
}
return 0;
}
```

### Set逆序排序

```cpp
set <int, greater<int> > setTest;
```

### 自定义set排序规则

在C++中，可以使用**`std::set`**容器来存储一组唯一的元素，其中元素的排序是由容器的比较函数所定义的。默认情况下，**`std::set`**使用**`std::less`**比较函数进行排序，即升序排列。

如果你想要自定义**`std::set`**的排序规则，可以通过提供自己的比较函数来实现。比较函数是一个二元谓词，它接受两个参数并返回一个**`bool`**类型值，用于指示它们的相对顺序。

例如，如果你有一个名为**`Person`**的类，并且你想按照人的年龄进行降序排序，可以编写一个比较函数如下：

```cpp

struct AgeComparator {
  bool operator()(const Person& p1, const Person& p2) const {
    return p1.getAge() > p2.getAge();
  }
};
```

这个比较函数定义了一个名为**`AgeComparator`**的结构体，其中包含一个重载的调用运算符**`operator()`**，它接受两个**`Person`**类型的参数，并比较它们的年龄大小。在这个比较函数中，如果第一个人的年龄比第二个人的年龄大，就返回**`true`**，否则返回**`false`**。

然后，你可以在定义**`std::set`**容器时，通过将自定义的比较函数对象作为第二个模板参数来指定你的排序规则。例如：

```cpp

std::set<Person, AgeComparator> people;

```

在这个例子中，**`std::set`**的元素类型是**`Person`**，排序规则是**`AgeComparator`**结构体中定义的比较函数对象。这将使得**`std::set`**容器根据人的年龄进行降序排序。

需要注意的是，你的比较函数必须定义为常量成员函数，以便可以在**`std::set`**内部进行调用。

```cpp
std::set<int, [](int a, int b) { return a > b; }> mySet;
```

### 自定义比较函数

```cpp
struct Node {
    int val;
    vector<int> times;
    
    Node(int v = 0): val(v) {}
};
struct cmp {
    bool operator()(const Node* n1, const Node* n2) const { 
        // cout << n1->times.size() << " " << n2->times.size() << endl;
        return n1->times.back() > n2->times.back();
    }
};
unordered_map<int, set<Node*, cmp>> freqs;
```



## 对于std::prev

- vector

如果调用**`std::prev(std::vector::begin())，`**并且**`std::vector`**为空，则会产生未定义的行为，因为尝试访问一个空容器的第一个元素之前的元素是未定义的。因此，在使用**`std::prev(std::vector::begin())`**之前，需要确保**`std::vector`**中至少有一个元素。

如果调用**`std::prev(std::vector::end())`**并且**`std::vector`**为空，则会产生未定义的行为，因为尝试访问一个空容器的最后一个元素是未定义的。因此，在使用**`std::prev(std::vector::end())`**
之前，需要确保**`std::vector`**中至少有一个元素。

- map/set

在C++中，**`prev(map.begin())`**会导致编译错误，因为**`map::begin()`**返回的是一个迭代器，它不能被递减。

而**`prev(map.end())`**会返回指向**`map`**中最后一个元素的迭代器，即**`map`**中的最后一个元素的前一个位置，这是因为**`map::end()`**返回的是指向容器“末尾元素下一个位置”的迭代器。因此，**`prev(map.end())`**是一个有效的操作，但需要注意，只有当**`map`**不为空时，**`prev(map.end())`**才是有效的。如果**`map`**为空，则该操作行为是未定义的。

需要注意的是，**`prev()`**函数在C++11中引入，在使用该函数之前需要包含头文件**`<iterator>`**。

- unordered_map

在C++中,prev(unordered_map.begin()), prev(unordered_map.end())会导致什么行为

在 C++ 中，**`prev(unordered_map.begin())`** 将会导致未定义行为（undefined behavior），因为 **`unordered_map`** 是一个哈希表，它的元素是无序的，因此不能保证其元素的顺序。因此，**`prev(unordered_map.begin())`** 不能保证返回哈希表中的前一个元素，因为没有定义“前一个元素”的概念。

类似地，**`prev(unordered_map.end())`** 也会导致未定义行为，因为 **`end()`** 返回一个指向哈希表中虚拟的“超出范围”的元素的迭代器，而在这个“超出范围”的元素上调用 **`prev()`** 是未定义行为。

如果想要获取 **`unordered_map`** 中的前一个或后一个元素，可以使用 **`++`** 或 **`--`** 运算符，或者使用 **`std::next`** 和 **`std::prev`** 函数来实现。但需要注意，这些操作都要在保证迭代器指向有效元素的前提下才能进行。