# Big Data

[toc]



## 1、如何从大量的 URL 中找出相同的 URL？ 

> How do you find the same URL from a large number of urls?

>  题目描述
>
> Description of the subject
>
> 给定 a、b 两个文件，各存放 50 亿个 URL，每个 URL 各占 64B，内存限制是 4G。请找出 a、b 两个文件共同的 URL。

### 解答思路 Solutions

#### 1. 分治策略 (Divide and conquer strategy)

每个 URL 占 64B，那么 50 亿个 URL 占用的空间大小约为 320GB。

At 64B per URL, that's about 320GB of space for 5 billion urls.

> 5, 000, 000, 000 _ 64B ≈ 5GB _ 64 = 320GB

由于内存大小只有 4G，因此，我们不可能一次性把所有 URL 加载到内存中处理。对于这种类型的题目，一般采用**分治策略**，即：把一个文件中的 URL 按照某个特征划分为多个小文件，使得每个小文件大小不超过 4G，这样就可以把这个小文件读到内存中进行处理了。

Since the memory size is only 4G, it is not possible to load all urls into memory for processing at once. For this type of problem, the general use of **divide-and-conquer** strategy, that is: the URL in a file according to a certain characteristic divided into many small files, so that each small file size not more than 4G, this allows the small file to be read into memory for processing.

**思路如下**：

首先遍历文件 a，对遍历到的 URL 求 `hash(URL) % 1000` ，根据计算结果把遍历到的 URL 存储到 a0, a1, a2, ..., a999，这样每个大小约为 300MB。使用同样的方法遍历文件 b，把文件 b 中的 URL 分别存储到文件 b0, b1, b2, ..., b999 中。这样处理过后，所有可能相同的 URL 都在对应的小文件中，即 a0 对应 b0, ..., a999 对应 b999，不对应的小文件不可能有相同的 URL。那么接下来，我们只需要求出这 1000 对小文件中相同的 URL 就好了。

First traversal file A, the URL traversal `hash(URL) % 1000`, according to the results of the calculation of the traverURL URL stored to a0, a1, a2, ... , a999, so that each size is approximately 300MB. Use the same method to traverse file b, file B to store the URL in file B0, B1, B2, ... B999. After this processing, all possibly the same urls are in the corresponding small file, that is, a 0 for B 0, ... , A999 corresponds to B999, and small files that do not correspond can not have the same URL. Next, we just need to ask for the same URL in the 1000 pairs of files.

接着遍历 ai( `i∈[0,999]` )，把 URL 存储到一个 HashSet 集合中。然后遍历 bi 中每个 URL，看在 HashSet 集合中是否存在，若存在，说明这就是共同的 URL，可以把这个 URL 保存到一个单独的文件中。

It then iterates through AI (i ∈[0,999]) and stores the URL in a HashSet collection. It then iterates through each URL in the BI to see if it exists in the HashSet collection. If it does, it's a common URL that can be saved to a separate file.

#### 2. 前缀树 (Prefix trees)

一般而言，URL 的长度差距不会不大，而且前面几个字符，绝大部分相同。这种情况下，非常适合使用**字典树**（trie tree） 这种数据结构来进行存储，降低存储成本的同时，提高查询效率。

In general, the length of the URL gap will not be small, and the first few characters, most of the same. In this case, it is very suitable to use the trie tree data structure to store, reduce the cost of storage, and improve the efficiency of query.

### 方法总结 Method summary

#### 分治策略 Divide and conquer strategy

1. 分而治之，进行哈希取余；
2. Divide and rule, hashing;
3. 对每个子文件进行 HashSet 统计。
4. HashSet statistics for each child file.

#### 前缀树 Prefix tree

1. 利用字符串的公共前缀来降低存储成本，提高查询效率。

   The common prefix of string is used to reduce the storage cost and improve the query efficiency.



## 2、如何从大量数据中找出高频词？ 

> How do you find high-frequency words in large amounts of data?

### 题目描述 Description of the subject

有一个 1GB 大小的文件，文件里每一行是一个词，每个词的大小不超过 16B，内存大小限制是 1MB，要求返回频数最高的 100 个词(Top 100)。

There is a 1GB file with one word per line and no more than 16B words per line. The memory size limit is 1MB and requires the highest frequency 100 words to be returned (Top 100) .

### 解答思路 Solutions

由于内存限制，我们依然无法直接将大文件的所有词一次读到内存中。因此，同样可以采用**分治策略**，把一个大文件分解成多个小文件，保证每个文件的大小小于 1MB，进而直接将单个小文件读取到内存中进行处理。

Due to memory limitations, we still can not read all the words of a large file directly into memory at once. Therefore, we can also adopt **divide-and-conquer strategy**, decompose a large file into many small files, ensure each file size less than 1MB, and then directly read a single small file into memory for processing.

**思路如下**：

首先遍历大文件，对遍历到的每个词 x，执行 `hash(x) % 5000` ，将结果为 i 的词存放到文件 ai 中。遍历结束后，我们可以得到 5000 个小文件。每个小文件的大小为 200KB 左右。如果有的小文件大小仍然超过 1MB，则采用同样的方式继续进行分解。

The first step is to traverse the large file, Hash (x)% 5000 for each word X traversed, and store the resulting I word in the file AI. After the traversal, we can get 5000 small files. Each small file is about 200 kb in size. If some of the small files are still larger than 1MB, continue to decompose in the same manner.

接着统计每个小文件中出现频数最高的 100 个词。最简单的方式是使用 HashMap 来实现。其中 key 为词，value 为该词出现的频率。具体方法是：对于遍历到的词 x，如果在 map 中不存在，则执行 `map.put(x, 1)` ；若存在，则执行 `map.put(x, map.get(x)+1)` ，将该词频数加 1。

Then count each small file in the highest frequency of 100 words. The easiest way to do this is to use a HashMap. Where key is the word and value is the frequency of occurrence of the word. Here's how: for the traversal word X, if it doesn't exist in the map, execute  `map.put(x, 1)`  ; if present, execute  `map.put(x, map.get(x)+1)`  , which adds the frequency of the word to 1.

上面我们统计了每个小文件单词出现的频数。接下来，我们可以通过维护一个**小顶堆**来找出所有词中出现频数最高的 100 个。具体方法是：依次遍历每个小文件，构建一个**小顶堆**，堆大小为 100。如果遍历到的词的出现次数大于堆顶词的出现次数，则用新词替换堆顶的词，然后重新调整为**小顶堆**，遍历结束后，小顶堆上的词就是出现频数最高的 100 个词。

Above we counted the frequency of each small file word. Next, we can find the 100 most frequent words by maintaining a `small top heap`. This is done by traversing each small file in turn to build a small top heap with a heap size of 100. If the number of occurrences of the words traversed is greater than the number of occurrences of the top words of the heap, replace the top words of the heap with new words, and then readjust to a small top heap, the words on the small top heap are the 100 most frequent words.

### 方法总结 Method summary

1. 分而治之，进行哈希取余；

   Divide and rule, hashing;

2. 使用 HashMap 统计频数；

   Use a HashMap to count frequency;

3. 求解**最大**的 TopN 个，用**小顶堆**；求解**最小**的 TopN 个，用**大顶堆**。

   For the largest TopN, use the small top heap; for the smallest TOPN, use the large top heap.



## 3、如何在大量的数据中找出不重复的整数？

> How do you find unduplicated integers in large amounts of data?

### 题目描述 Description

在 2.5 亿个整数中找出不重复的整数。注意：内存不足以容纳这 2.5 亿个整数。

Find unrepeated integers in 250 million integers. Note: There is not enough memory to hold these 250 million integers.

### 解答思路 Solutions

#### 方法一：分治法 (Divide and conquer)

与前面的题目方法类似，先将 2.5 亿个数划分到多个小文件，用 HashSet/HashMap 找出每个小文件中不重复的整数，再合并每个子结果，即为最终结果。

Similar to the previous topic method, divide 250 million numbers into smaller files, use a HashSet/HashMap to find the unduplicated integers in each small file, and then merge each subresult to get the final result.

#### 方法二：位图法 (Bitmap method)

**位图**，就是用一个或多个 bit 来标记某个元素对应的值，而键就是该元素。采用位作为单位来存储数据，可以大大节省存储空间。

Bitmaps, where one or more bits are used to mark the value of an element, and the key is that element. Using bits as units to store data can save storage space greatly.

位图通过使用位数组来表示某些元素是否存在。它可以用于快速查找，判重，排序等。

Bitmaps use an array of bits to indicate whether certain elements exist. It can be used for fast search, weight, sort and so on.

假设我们要对 `[0,7]` 中的 5 个元素 (6, 4, 2, 1, 5) 进行排序，可以采用位图法。0~7 范围总共有 8 个数，只需要 8bit，即 1 个字节。首先将每个位都置 0：

Suppose we want to sort the five elements (6,4,2,1,5) in [0,7] , using bitmap. There are 8 numbers in the 0-7 range, requiring only 8 bits, or 1 byte. First set each position to 0:

```text
0 0 0 0 0 0 0 0
```

然后遍历 5 个元素，首先遇到 6，那么将下标为 6 的位的 0 置为 1；接着遇到 4，把下标为 4 的位 的 0 置为 1：

It then iterates through the five elements, first coming up to 6, so that the 0 of the 6 bit is set to 1; then it comes up to 4, so that the 0 of the 4 bit is set to 1:

```text
0 0 0 0 1 0 1 0
```

依次遍历，结束后，位数组是这样的：

At the end, the array of bits looks like this:

```text
0 1 1 0 1 1 1 0
```

每个为 1 的位，它的下标都表示了一个数：

Each bit is 1, and its subscript represents a number:

```text
for i in range(8):
    if bits[i] == 1:
        print(i)
```

这样我们其实就已经实现了排序。

So we've actually implemented sorting.

对于整数相关的算法的求解，**位图法**是一种非常实用的算法。假设 int 整数占用 4B，即 32bit，那么我们可以表示的整数的个数为 $2^32$。

For integer correlation algorithm, `bitmap method` is a very practical algorithm. Assuming an int integer takes 4b, or 32 bits, the number of integers we can represent is $2 ^ 32 $.

**那么对于这道题**，我们用 2 个 bit 来表示各个数字的状态：

So for this problem, let's use two bits to represent the state of each number:

- 00 表示这个数字没出现过；
- 00 means that this number has not appeared;
- 01 表示这个数字出现过一次（即为题目所找的不重复整数）；
- 01 means that this number has been found once (that is, for the title of the non-repeating integer) ;
- 10 表示这个数字出现了多次。
- 10 means that the number appears many times.

那么这 $2^{32}$ 个整数，总共所需内存为 $2^{32}$*2b=1GB。因此，当可用内存超过 1GB 时，可以采用位图法。假设内存满足位图法需求，进行下面的操作：

So this $2 ^ {32} $Integer, the total memory required is $2 ^ {32} $* 2B = 1GB. Thus, bitmap methods can be used when the available memory exceeds 1GB. Assuming memory meets bitmap requirements, do the following:

遍历 2.5 亿个整数，查看位图中对应的位，如果是 00，则变为 01，如果是 01 则变为 10，如果是 10 则保持不变。遍历结束后，查看位图，把对应位是 01 的整数输出即可。

Traversing 250 million integers, look at the corresponding bits in the bitmap, becoming 01 if 00,10 if 01, and remaining unchanged if 10. After the end of traversal, view bitmap, the corresponding bit is 01 integer output can.

当然，本题中特别说明：**内存不足以容纳这 2.5 亿个整数**，2.5 亿个整数的内存大小为：2.5e8/1024/1024/1024 * 4=3.72GB， 如果内存大于 1GB，是可以通过位图法解决的。

Of course, the special note in this topic: memory is not enough to hold these 250 million integers, 250 million integers memory size is: 2.5e8/1024/1024/1024 * 4 = 3.72 gb, if memory is greater than 1GB, can be solved by bitmap method

### 方法总结 Method summary

**判断数字是否重复的问题**，位图法是一种非常高效的方法，当然前提是：内存要满足位图法所需要的存储空间。

To determine whether the number is repeated, bitmap method is a very efficient method, of course, the premise is: **Memory should meet the needs of bitmap storage space**



## 4、如何在大量的数据中判断一个数是否存在？

> How do you determine whether a number exists in a large amount of data?

### 题目描述 Description

给定 40 亿个不重复的没排过序的 unsigned int 型整数，然后再给定一个数，如何快速判断这个数是否在这 40 亿个整数当中？

Given 4 billion unsigned, unsorted integers that don't repeat, and then given a number, how do you quickly determine if that number is one of those 4 billion integers?

### 解答思路 Solutions

#### 方法一：分治法 (Divide and conquer)

#### 方法二：位图法 (Bitmap method)

由于 unsigned int 数字的范围是 `[0, 1 << 32)`，我们用 `1<<32=4,294,967,296` 个 bit 来表示每个数字。初始位均为 0，那么总共需要内存：4,294,967,296bit ≈ 16KB。

Since the range of unsigned int numbers is  `[0, 1 << 32)`, we represent each number with `1<<32=4,294,967,296`  bits. The initial bits are all 0, so the total memory required is: 4,294,967,296bit ≈ 16KB.

我们读取这 40 亿个整数，将对应的 bit 设置为 1。接着读取要查询的数，查看相应位是否为 1，如果为 1 表示存在，如果为 0 表示不存在。

We read the 4 billion integers and set the corresponding bit to 1. It then reads the number to query and checks to see if the corresponding bit is 1, if it is 1 it exists, and if it is 0 it does not.

### 方法总结 Method summary

**判断数字是否存在、判断数字是否重复的问题**，位图法是一种非常高效的方法。

The bitmap method is a very efficient method to determine whether the number exists and whether the number is repeated



## 5、如何查询最热门的查询串？ 

> How to find the most popular query string?

### 题目描述 Description

搜索引擎会通过日志文件把用户每次检索使用的所有查询串都记录下来，每个查询串的长度不超过 255 字节。

The search engine will log all the query strings used by the user each time, and the length of each query string does not exceed 255 bytes.

假设目前有 1000w 个记录（这些查询串的重复度比较高，虽然总数是 1000w，但如果除去重复后，则不超过 300w 个）。请统计最热门的 10 个查询串，要求使用的内存不能超过 1G。（一个查询串的重复度越高，说明查询它的用户越多，也就越热门。）

Assume that there are currently 1000W records (these query strings have high repeatability, although the total is 1000W, but not more than 300W if duplicates are removed) . Please count the 10 most popular query strings, the required memory can not exceed 1GB. The higher the repeatability of a query string, the more users query it, and the more popular it becomes

### 解答思路 Solutions

每个查询串最长为 255B，1000w 个串需要占用 约 2.55G 内存，因此，我们无法将所有字符串全部读入到内存中处理。

Each query string can be up to 255B, and 1000W strings require about 2.55 GB of memory, so we can not read all the strings into memory for processing.

#### 方法一：分治法 (Divide and conquer)

分治法依然是一个非常实用的方法。

Divide and conquer is still a very practical method.

划分为多个小文件，保证单个小文件中的字符串能被直接加载到内存中处理，然后求出每个文件中出现次数最多的 10 个字符串；最后通过一个小顶堆统计出所有文件中出现最多的 10 个字符串。

It is divided into several small files to ensure that the strings in a single small file can be loaded directly into the memory for processing, and then find out the maximum number of 10 strings in each file; Finally, a small top heap counts the 10 strings that occur most in all the files.

方法可行，但不是最好

The method works, but is not the best

#### 方法二：HashMap 法 

虽然字符串总数比较多，但去重后不超过 300w，因此，可以考虑把所有字符串及出现次数保存在一个 HashMap 中，所占用的空间为 300w*(255+4)≈777M（其中，4 表示整数占用的 4 个字节）。由此可见，1G 的内存空间完全够用。

Although the total number of strings is high, it does not exceed 300W after deduplication, so you might consider saving all strings and occurrences in a HashMap, the space occupied is 300W * (255 + 4)≈777M (where 4 represents the 4 bytes of an integer) . As you can see, 1gb of memory is more than enough.

首先，遍历字符串，若不在 map 中，直接存入 map，value 记为 1；若在 map 中，则把对应的 value 加 1，这一步时间复杂度 `O(N)` 。

First, traverse the string, if not in the map, directly into the map, value as 1; if in the map, the corresponding value plus 1, this step time complexity O (N) .

接着遍历 map，构建一个 10 个元素的小顶堆，若遍历到的字符串的出现次数大于堆顶字符串的出现次数，则进行替换，并将堆调整为小顶堆。

The map is then traversed to build a 10-element small-top heap. If the number of occurrences of the traversed string is greater than the number of occurrences of the heap-top string, replace it and adjust the heap to a small-top heap.

遍历结束后，堆中 10 个字符串就是出现次数最多的字符串。这一步时间复杂度 `O(Nlog10)` 。

At the end of the traversal, the 10 strings in the heap are the ones that appear the most. This step time complexity is `O(Nlog10)`

#### 方法三：前缀树法 Method Three: prefix tree method

方法二使用了 HashMap 来统计次数，当这些字符串有大量相同前缀时，可以考虑使用前缀树来统计字符串出现的次数，树的结点保存字符串出现次数，0 表示没有出现。

Method 2 uses a HashMap to count the occurrences. When these strings have a large number of the same prefixes, consider using a prefix tree to count the occurrences of the strings. The nodes of the tree hold the occurrences of the strings, 0 indicates no presence.

在遍历字符串时，在前缀树中查找，如果找到，则把结点中保存的字符串次数加 1，否则为这个字符串构建新结点，构建完成后把叶子结点中字符串的出现次数置为 1。

When traversing a string, look in the prefix tree and, if found, add the number of times the string was saved in the node by 1, otherwise build a new node for the string, when the build is complete, set the number of occurrences of the string in the leaf node to 1.

最后依然使用小顶堆来对字符串的出现次数进行排序。

Finally, the small top heap is still used to sort the number of occurrences of the string.

### 方法总结 Method summary

前缀树经常被用来统计字符串的出现次数。它的另外一个大的用途是字符串查找，判断是否有重复的字符串等。

Prefix trees are often used to count the occurrence of strings. Another big use of it is string lookup, judging whether there are duplicate strings, and so on



## 6、如何统计不同电话号码的个数？ 

> How do I count different phone numbers?

### 题目描述 Description

已知某个文件内包含一些电话号码，每个号码为 8 位数字，统计不同号码的个数。

It is known that a file contains telephone numbers, each of which is 8 digits, counting the number of different numbers.

### 解答思路 Solution

这道题本质还是求解**数据重复**的问题，对于这类问题，一般首先考虑位图法。

The essence of this problem is to solve the problem of data duplication, for this kind of problem, generally first consider bitmap method.

对于本题，8 位电话号码可以表示的号码个数为 $10^8$ 个，即 1 亿个。我们每个号码用一个 bit 来表示，则总共需要 1 亿个 bit，内存占用约 12M。

For this problem, 8-digit telephone numbers can be represented by the number of  $10^8$ numbers, that is, 100 million. Each of our numbers is represented by a bit, which would require a total of 100 million bits and about 12 megabytes of memory.

**思路如下**：

申请一个位图数组，长度为 1 亿，初始化为 0。然后遍历所有电话号码，把号码对应的位图中的位置置为 1。遍历完成后，如果 bit 为 1，则表示这个电话号码在文件中存在，否则不存在。bit 值为 1 的数量即为 不同电话号码的个数。

Apply a bitmap array, 100 million in length, initialized to 0. Then iterate through all the phone numbers, putting the position 1 in the corresponding bitmap. When the traversal is complete, a bit of 1 indicates that the phone number exists in the file, otherwise it does not. The number of bits is the number of different phone numbers.

### 方法总结 Method summary

求解数据重复问题，记得考虑位图法。

For data duplication, remember to consider the bitmap method.



## 7、 如何从 5 亿个数中找出中位数？

### 题目描述

从 5 亿个数中找出中位数。数据排序后，位置在最中间的数就是中位数。当样本数为奇数时，中位数为 第 `(N+1)/2` 个数；当样本数为偶数时，中位数为 第 `N/2` 个数与第 `1+N/2` 个数的均值。

### 解答思路

如果这道题没有内存大小限制，则可以把所有数读到内存中排序后找出中位数。但是最好的排序算法的时间复杂度都为 `O(NlogN)` 。这里使用其他方法。

#### 方法一：双堆法

维护两个堆，一个大顶堆，一个小顶堆。大顶堆中最大的数**小于等于**小顶堆中最小的数；保证这两个堆中的元素个数的差不超过 1。

若数据总数为**偶数**，当这两个堆建好之后，**中位数就是这两个堆顶元素的平均值**。当数据总数为**奇数**时，根据两个堆的大小，**中位数一定在数据多的堆的堆顶**。

> 见 LeetCode No.295：https://leetcode.com/problems/find-median-from-data-stream/

以上这种方法，需要把所有数据都加载到内存中。当数据量很大时，就不能这样了，因此，这种方法**适用于数据量较小的情况**。5 亿个数，每个数字占用 4B，总共需要 2G 内存。如果可用内存不足 2G，就不能使用这种方法了，下面介绍另一种方法。

#### 方法二：分治法

分治法的思想是把一个大的问题逐渐转换为规模较小的问题来求解。

对于这道题，顺序读取这 5 亿个数字，对于读取到的数字 num，如果它对应的二进制中最高位为 1，则把这个数字写到 f1 中，否则写入 f0 中。通过这一步，可以把这 5 亿个数划分为两部分，而且 f0 中的数都大于 f1 中的数（最高位是符号位）。

划分之后，可以非常容易地知道中位数是在 f0 还是 f1 中。假设 f0 中有 1 亿个数，那么中位数一定在 f1 中，且是在 f1 中，从小到大排列的第 1.5 亿个数与它后面的一个数的平均值。

> **提示**，5 亿数的中位数是第 2.5 亿与右边相邻一个数求平均值。若 f1 有一亿个数，那么中位数就是 f0 中从第 1.5 亿个数开始的两个数求得的平均值。

对于 f0 可以用次高位的二进制继续将文件一分为二，如此划分下去，直到划分后的文件可以被加载到内存中，把数据加载到内存中以后直接排序，找出中位数。

> **注意**，当数据总数为偶数，如果划分后两个文件中的数据有相同个数，那么中位数就是数据较小的文件中的最大值与数据较大的文件中的最小值的平均值



## 8、如何按照 query 的频度排序？ 

> How do you rank queries by frequency?

### 题目描述 Description

有 10 个文件，每个文件大小为 1G，每个文件的每一行存放的都是用户的 query，每个文件的 query 都可能重复。要求按照 query 的频度排序。

There are 10 files, each of which is 1GB in size, and each line of each file holds the user's query, which can be duplicated for each file. Requests are sorted by query frequency.

### 解答思路 Solutions

如果 query 的重复度比较大，可以考虑一次性把所有 query 读入内存中处理；如果 query 的重复率不高，那么可用内存不足以容纳所有的 query，这时候就需要采用分治法或其他的方法来解决。

If your query has a high repetition rate, consider reading all your queries into memory at once; if your query doesn't have a high repetition rate, then the available memory isn't big enough to hold all your queries, this time needs to adopt divide and conquer law or other method to solve.

#### 方法一：HashMap 法 (hashmap method)

如果 query 重复率高，说明不同 query 总数比较小，可以考虑把所有的 query 都加载到内存中的 HashMap 中。接着就可以按照 query 出现的次数进行排序。

If the query repetition rate is high and the total number of different queries is small, consider loading all your queries into an in-memory HashMap. You can then sort by the number of times query appears.

#### 方法二：分治法 (Divide and conquer)

分治法需要根据数据量大小以及可用内存的大小来确定问题划分的规模。对于这道题，可以顺序遍历 10 个文件中的 query，通过 Hash 函数 `hash(query) % 10` 把这些 query 划分到 10 个小文件中。之后对每个小文件使用 HashMap 统计 query 出现次数，根据次数排序并写入到另外一个单独文件中。

Divide-and-conquer requires that the size of the problem be determined by the size of the amount of data and the amount of memory available. For this problem, you can iterate through the 10 files in the query sequence, using the Hash function  `hash(query) % 10`  divide the query into 10 small files. The HashMap is then used to count the query occurrences for each small file, sort them by number, and write them to a separate file.

接着对所有文件按照 query 的次数进行排序，这里可以使用归并排序（由于无法把所有 query 都读入内存，因此需要使用外排序）。

Then sort all the files by the number of times they were queried, using merge sort (since you can't read all queries into memory, you need to sort them out) .

### 方法总结 Method summary

- 内存若够，直接读入进行排序；

  If memory is enough, read directly to sort;

- 内存不够，先划分为小文件，小文件排好序后，整理使用外排序进行归并。

  Memory is not enough, first divided into small files, small files sorted, collated using the outer sort merge



### 补充： 外排序之外归并排序

外排序的一个例子是外归并排序（External merge sort），它读入一些能放在内存内的数据量，在内存中排序后输出为一个顺串（即是内部数据有序的临时文件），处理完所有的数据后再进行归并。[1][2]比如，要对900 MB的数据进行排序，但机器上只有100 MB的可用内存时，外归并排序按如下方法操作：

1. 读入100 MB的数据至内存中，用某种常规方式（如快速排序、堆排序、归并排序等方法）在内存中完成排序。

2. 将排序完成的数据写入磁盘。
3. 读入每个临时文件（顺串）的前10 MB（ = 100 MB / (9块 + 1)）的数据放入内存中的输入缓冲区，最后的10 MB作为输出缓冲区。（实践中，将输入缓冲适当调小，而适当增大输出缓冲区能获得更好的效果。）
4. 执行九路归并算法，将结果输出到输出缓冲区。一旦输出缓冲区满，将缓冲区中的数据写出至目标文件，清空缓冲区。一旦9个输入缓冲区中的一个变空，就从这个缓冲区关联的文件，读入下一个10M数据，除非这个文件已读完。这是“外归并排序”能在主存外完成排序的关键步骤 -- 因为“归并算法”(merge algorithm)对每一个大块只是顺序地做一轮访问(进行归并)，每个大块不用完全载入主存。

为了增加每一个有序的临时文件的长度，可以采用置换选择排序（Replacement selection sorting）。它可以产生大于内存大小的顺串。具体方法是在内存中使用一个最小堆进行排序，设该最小堆的大小为M。算法描述如下：

1. 初始时将输入文件读入内存，建立最小堆。
2. 将堆顶元素输出至输出缓冲区。然后读入下一个记录：
3. 若该元素的关键码值不小于刚输出的关键码值，将其作为堆顶元素并调整堆，使之满足堆的性质；
4. 否则将新元素放入堆底位置，将堆的大小减1。
5. 重复第2步，直至堆大小变为0。
6. 此时一个顺串已经产生。将堆中的所有元素建堆，开始生成下一个顺串。

此方法能生成平均长度为2M的顺串，可以进一步减少访问外部存储器的次数，节约时间，提高算法效率。

高效的外排序所耗的时间依然是`O(NlogN)`。若利用好现在计算机上GB的内存，可使时间复杂度中的对数项增长比较缓慢。