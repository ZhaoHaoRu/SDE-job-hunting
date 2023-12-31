# 笔试&面试中遇到的知识点

## C++

#### 返回值优化

返回值优化，顾名思义，就是与返回值有关的优化，是当函数是按值返回（而不是引用、指针）时，为了避免产生不必要的临时对象以及值拷贝而进行的优化。

#### 友元

除了[友元函数](https://so.csdn.net/so/search?q=友元函数&spm=1001.2101.3001.7020)，还有友元类，两者统称为友元（friend）。

借助友元，可以使得普通函数或其他类中的成员函数可以访问某个类的私有成员和保护成员。

- 友元函数：普通函数可以访问某个类私有成员或保护成员。
- 友元类：类A中的成员函数可以访问类B中的私有或保护成员。