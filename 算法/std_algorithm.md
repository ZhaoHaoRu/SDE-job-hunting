# Algorithm

## nth_element

> 含义：当采用默认的升序排序规则（std::less<T>）时，该函数可以从某个序列中找到第 n 小的元素 K，并将 K 移动到序列中第 n 的位置处。不仅如此，整个序列经过 nth_element() 函数处理后，所有位于 K 之前的元素都比 K 小，所有位于 K 之后的元素都比 K 大。
> 

**nth_element() 函数只适用于 array、vector、deque 这 3 个容器。**

### 语法格式

```cpp
//排序规则采用默认的升序排序
void nth_element (RandomAccessIterator first,
                  RandomAccessIterator nth,
                  RandomAccessIterator last);
//排序规则为自定义的 comp 排序规则
void nth_element (RandomAccessIterator first,
                  RandomAccessIterator nth,
                  RandomAccessIterator last,
                  Compare comp);
```

其中，各个参数的含义如下：

- first 和 last：都是随机访问迭代器，[first, last) 用于指定该函数的作用范围（即要处理哪些数据）；
- nth：也是随机访问迭代器，其功能是令函数查找“第 nth 大”的元素，并将其移动到 nth 指向的位置；
- comp：用于自定义排序规则。

### 使用实例

```cpp
//以函数对象的方式自定义排序规则
class mycomp2 {
public:
    bool operator() (int i, int j) {
        return (i > j);
    }
};
int main() {
    std::vector<int> myvector{3,1,2,5,4};
    //默认的升序排序作为排序规则
    std::nth_element(myvector.begin(), myvector.begin()+2, myvector.end());
    cout << "第一次nth_element排序：\n";
    for (std::vector<int>::iterator it = myvector.begin(); it != myvector.end(); ++it) {
        std::cout << *it << ' ';
    }
    //自定义的 mycomp2() 或者 mycomp1 降序排序作为排序规则
    std::nth_element(myvector.begin(), myvector.begin() + 3, myvector.end(),mycomp1);
    cout << "\n第二次nth_element排序：\n";
    for (std::vector<int>::iterator it = myvector.begin(); it != myvector.end(); ++it) {
        std::cout << *it << ' ';
    }
    return 0;
}
```

### 时间复杂度

时间复杂度详见CPPreference

## 获取随机数

- [0, x)的随机整数：`rand() % x`

- [a, b)的随机整数：`rand() % (b - a) + a`
- [a, b]的随机整数：`rand() % (b - a + 1) + a`
- (a, b]的随机整数：`rand() % (b - a) + a + 1`



## Sort

### 最常用的用法

```cpp
sort(array.begin(), array.end());
```

### 使用标准库

```cpp
// 用标准库比较函数对象排序
std::sort(s.begin(), s.end(), std::greater<int>());
```

### 自定义排序方法

```cpp
struct {
    bool operator()(const vector<int> &p1,const vector<int> &p2){
        return p1[1] < p2[1] || (p1[1] == p2[1] && p1[0] > p2[0]);
    }
} comp;
sort(intervals.begin(), intervals.end(), comp);
```

false才是保持顺序不变



### partial_sort

**`partial_sort`** 是 C++ 标准库中的一个算法，它能够在保持前 $k$ 个元素有序的情况下将序列的所有元素进行排序。

**`partial_sort`** 函数的定义如下：

```
cppCopy code
template<class RandomIt>
void partial_sort(RandomIt first, RandomIt middle, RandomIt last);

template<class RandomIt, class Compare>
void partial_sort(RandomIt first, RandomIt middle, RandomIt last, Compare comp);

```

其中，**`first`** 和 **`last`** 分别指向需要进行排序的区间的起始和结束位置，**`middle`** 指向区间中第 $k$ 个元素的位置，排序结果将会保证前 $k$ 个元素是整个区间中最小的 $k$ 个元素。

需要注意的是，由于 **`partial_sort`** 的特殊性质，它并不能保证前 $k$ 个元素是唯一的。如果序列中有多个元素的值相等，那么这些元素都可能出现在前 $k$ 个位置。

**`partial_sort`** 的时间复杂度为 $O(n \log k)$，其中 $n$ 是需要排序的元素的总数。这个时间复杂度远远优于将整个序列进行排序的 $O(n \log n)$，因此在需要找出前 $k$ 大或前 $k$ 小元素的场景下，使用 **`partial_sort`** 可以带来更好的性能。max_element(min_element的用法是一样的）

寻找范围 `[first, last)` 中的最大元素。

注意返回值是迭代器类型

### 例子

```cpp
static bool abs_compare(int a, int b)
{
    return (std::abs(a) < std::abs(b));
}

int main()
{
	std::vector<int> v{ 3, 1, -14, 1, 5, 9 };
	std::vector<int>::iterator result;
	
	result = std::max_element(v.begin(), v.end());
	std::cout << "max element at: " <<std::distance(v.begin(), result) << '\n';
	
	result = std::max_element(v.begin(), v.end(), abs_compare);
	std::cout << "max element (absolute) at: " <<std::distance(v.begin(), result) << '\n';
}
```



## **minmax_element**

寻找范围 `[first, last)`中最小和最大的元素。

### 例子

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main()
{
		std::vector<int> v = { 3, 9, 1, 4, 2, 5, 9 };
		auto result = std::minmax_element(v.begin(), v.end());
		std::cout << "min element at: " << (result.first - v.begin()) << '\n';
		std::cout << "max element at: " << (result.second - v.begin()) << '\n';
}
```

## lexicographical_compare字符串排序算法

lexicographical_compare()算法可以比较由开始和结束迭代器定义的两个序列。它的前两个参数定义了第一个序列，第 3 和第 4 个参数分别是第二个序列的开始和结束迭代器。默认用 < 运算符来比较元素，但在需要时，也可以提供一个实现小于比较的函数对象作为可选的第 5 个参数。如果第一个序列的字典序小于第二个，这个算法会返回 true，否则返回 false。所以，返回 false 表明第一个序列大于或等于第二个序列。

### 例子

```cpp
std::vector<string> phrase1 {"the", "tigers", "of", "wrath"};
std::vector<string> phrase2 {"the", "horses", "of", "instruction"};
auto less = std::lexicographical_compare (std::begin (phrase1), std: :end (phrase1),
std::begin(phrase2), std::end(phrase2)); std::copy(std::begin(phrase1), std::end(phrase1), std::ostream_iterator<string>{std::cout, " "});
std::cout << (less ? "are":"are not") << " less than ";
std::copy(std::begin(phrase2), std::end(phrase2), std::ostream_iterator <string>{std::cout, " "});
std::cout << std::endl;
```



## lower_bound

指向范围 [first, last) 中首个不满足 element < value 或 comp(element, value) 的元素的迭代器，或者在找不到这种元素时返回 last

lower_bound() 函数用于在指定区域内查找不小于目标值的第一个元素。

```cpp
std::vector<int> data = {1, 2, 4, 5, 5, 6};
 
for (int i = 0; i < 8; ++i)
{
    // 搜索首个不小于 i 的元素
    auto lower = std::lower_bound(data.begin(), data.end(), i);

    std::cout << i << " ≤ ";
    lower != data.end()
        ? std::cout << *lower << " 位于索引 " << std::distance(data.begin(), lower)
        : std::cout << "没有找到";
    std::cout << '\n';
}
```

- 自定义比较函数

```cpp
const auto cmp = [](const pair<int, int> &p1, const pair<int, int> &p2) {
            return p1.first < p2.first;
};
auto pos = lower_bound(storage[index].begin(), storage[index].end(), pair<int, int>(snap_id + 1, 0), cmp);
```



## 求最大公约数

```cpp
int gcd(int a, int b) { return b ? gcd(b, a % b) : a; }
```

- **最小公倍数=两数的乘积/最大公约（因）数**

