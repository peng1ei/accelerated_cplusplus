#C++记录

#ch01
##string使用
1. 头文件 <string>
2. string构造的几种方法
- std::string str(3, 'a'); // 使用构造函数, 用‘a’填充3次
- std::string str = "hello"; // 定义的同时初始化
- std::string str1 = str; // 使用赋值操作符, 重载了操作符 =
- std::string str2; // 定义str2，使用默认的初始化

3. string的 operator+操作符(左结合，left associative)允许的情况:
- a string + a string literal 或 a string literal + a string
- a string + a string
- a string literal + a string literal // error

4. string::size_type 获取string的大小

## std::cin 缓冲区
1. 标准I/O库中为了提高效率，在内存中使用了缓冲区机制，对于文件操作来说，使用全缓冲；
对于终端来说，使用行缓冲，而对于标准错误输出来说，一般不使用缓冲。
2. 导致缓冲区被 flush 的事件：
- 缓冲区已满，自动flush；
- 要求从标准输入设备中读取输入，这时，I/O库会立即flush缓冲区，而不必等到缓冲区满；
- 显式的 flush 缓冲区。

3. std::cin 从标准输入设备中读取一个string的时候，实际上只会读取一个单词，会丢弃空白
字符（空格、制表符、backspace或者换行符），然后读取输入的字符，直到遇到另一个空白字符或
文件结尾或行尾。
4.  the std::cin checks the buffer corresponding to the std::string variable.
- **If there is at least one word stored inside the buffer**, it flushes the first word from the buffer, read and assign that word to the std::string variable. The program does not need to pause and ask the user to supply values.
- **If the buffer is empty**, the program would pause and ask the user to supply values (words) to the buffer. Only when there are words inside the buffer, the std::cin would then read from it and assign the value to the std::string variable (and flush that word away from the buffer as a consequence).

eg:
	std::cout << "What is your name? ";
	std::string name;
	std::cin >> name; // setp 1 cin
	std::cout << "hello, " << name << std::endl;

	std::cin >> name; // setp 2 cin
	std::cout << "hello, " << name << "nice to meet you!" << std::endl;

Output:
buferr 初始状态为空, 当std::cin读取string时，此时要求用户在标准输入中输入内容：
------------------------------------------------------------
-                                                          -
------------------------------------------------------------

What is your name?peng lei zhang shan

当输入完"peng lei zhang shan" 时，buffer的内容如下：
------------------------------------------------------------
- peng lei zhang shan									   -
------------------------------------------------------------

当按完回车时：
setp 1 cin：从buffer获取 peng ，并将其从buffer清除，此时的 buffer内容如下：
------------------------------------------------------------
- lei zhang shan      									   -
------------------------------------------------------------

setp 2 cin：从buffer获取 lei，并将其从 buffer清除，此时的buffer内容如下：
------------------------------------------------------------
- zhang shan      									   -
------------------------------------------------------------
注意：如果后续还有 std::cin，则当zhang 和 shan被读完时，此时标准输入会停下来，等待用户继续输入
字符串，因为此时的buffer没有内容了，需要从标准输入中再输入。

4. std::cin 可以作为判断条件和循环条件，例如 if (cin >> name) 或者 while (cin >> grade)，当输入有效时，返回true，
当输入无效时，返回false。
因为 std::cin 是 istream 类型的, istream提供了将 cin 转化为 bool 的能力。

The value that this conversion yields depends on the internal state of the istream
object, which will remember whether the last attempt to read worked. Thus, using cin as a
condition is equivalent to testing whether the last attempt to read from cin was successful.
cin转换为bool是否成功依赖于istream内部的状态，如果发生以下三种情况之一时，则stream发生错误，cin转换为bool时
即为false:
- 到达文件结尾
- 输入的值与 cin 接收的变量的类型不一致
- 系统检测到硬件错误

#ch3
## vector
1. 使用	C++标准库时，标准库不仅实现基本的功能，而且还有性能上的保证，一般来说，性能都是可以的。
2. sort 排序的性能，在C++标准实现中，要求性能不低于 nlog(n)。

#ch4
1. 在头文件中一般不使用 using 声明语句。


#ch5
1. vector 删除操作很慢，因为内部用数组实现
2. 只要不是在 vector 末尾，进行插入和删除(erase)操作效率就不高
3. Thus, although i does not change, erase has the effect of adjusting the index to
denote the next element in the vector , which means that we must not increment it for the next
iteration. 在使用 vector 删除元素时，在删除元素后，vector的大小变了，就不能再继续使用之前的迭代器进行后续的操作。 

4. Iterator(迭代器) -- To that end, the C++ library supplies an assortment of types called iterators, which allow
access to data structures in ways that the library can control. This control lets the library ensure
efficient implementation.

5. 迭代器类型 - Iterator types，每个标准的容器都包含以下两种迭代器：
- container-type::const_iterator // 想改变容器里的元素时
- container-type::iterator // 对容器里的元素做只读操作
注意：可以将 iterator 转换为const_iterator（自动或非自动转换）；但是无法将const_iterator转换为 iterator。

对于迭起器，我们只需要知道（1）如何使用迭代器；（2）迭代器允许那些操作。

6. 迭代器的操作 - Iterator operations
- iter != students.end() // compare 操作
- ++iter // 自增操作，迭代器指向下一个元素
- (*iter).name // dereference 解引用，访问迭代器所在位置的元素*
- iter->name // 迭代器可以作为指向元素的指针

7. 有些容器(container)支持 random-access indexing，有些容器不支持 random-access indexing。
即 students.begin() + i，其中i为整数，则有些容器支持 + 操作，则有些容器不支持 + 操作，只支持顺序访问。

8. vector 支持 index操作，而 list 不支持 index操作。
9. vector使用迭代器删除元素时，则迭代器对后续的元素会失效，因为后续元素会前移；对于 list，使用迭代器删除元素时，迭代器不会失效，对后续的元素还有效？
10. 不能使用通用的 sort 对list排序，list有自己的排序成员函数 sort。
11. erase删除迭代器指定的元素，并返回删除元素的下一个迭代器。

## string
1. string 也可以看做一个容器，只是里面只存放字符，许多操作类似于vector，如支持index访问，支持迭代器访问
2. substr(index, length) 截取字符串
3. sting提供 getline() 函数，该函数有两个参数，一个是获取源，一个是存放获取字符串的地方，如 getline(cin, adderss)，返回获取源，如返回cin，所以可以使用返回值做条件测试语句，遇到行尾时则为false。

4. string str = string(i, j); // i, j 为某个容器的迭代器，截取 [i, j)位置的字符串

12. <cctype>里面包含了一些处理 单个字符 的函数，第一个c表示c++从c中继承过来,如函数 isspace() 就是判断字符是否为空白字符（包含tab、space、\n等）。

13. 顺序容器（sequence）：vector、list、string

#ch6 标准算法库 <algorithm>
1. copy http://www.cplusplus.com/reference/algorithm/copy/
2. find_if http://www.cplusplus.com/reference/algorithm/find_if/
3. 迭代器适配器：back_inserter http://www.cplusplus.com/reference/iterator/back_inserter/?kw=back_inserter
4. * 作为解引用时，其优先级和自增操作符 ++ 相同，且结合方向都是从右到左，所以以下表达式：
	while (begin != end) // 其中begin和end均为迭代器
		*out++ = *begin++; // out为迭代器

	则 *out++ 实际上为 *(out++) -> {*out; out++;}，所以:
		*out++ = *begin++ 等价于 { *out = *begin; ++out; ++begin; }
	it = begin++; 等价于 it = begin; ++begin;

5. 迭代器适配器（iterator adaptors）：
Let's return to iterator adaptors, which are functions that yield iterators with properties that
are related to their arguments in useful ways. The iterator adaptors are defined in <iterator> .
The most common iterator adaptor is back_inserter , which takes a container as its argument
and yields an iterator that, when used as a destination, appends values to the container.

6. 回文（palindrome）数：如 1234321、 1234432`；回文单词：如 civic、eye、madam、rotor。即正读反读都可以。
	如何判断一个 word 为回文单词?
	bool is_palindrome(const string &s) {
		return equal(s.begin(), s.end(), s.rbegin()); // 使用STL标准库里的算法
	}
	
	如何判断一个 number 为回文数？(先将 number 转换为 string，然后再调用 is_palindrome 函数)
	bool is_palindrome(int num) {
		string str = std::to_string(num); // to_string 在 <string>中
		return is_palindrome(str);
	}

7. find：如果查找失败，通常返回第二个参数（迭代器）
8. Algorithms act on container elements—they do not act on containers.
	算法作用于容器元素，而不作用于容器。而连接算法和容器元素的正是迭代器这个东西。因此可以说迭代器起了一个桥梁的作用。
		算法 <-------> 迭代器 <-------> 容器(元素)


#ch7 关联容器（associative containers） -- 提供高效的 look-up 操作
1. associative container. Such
containers automatically arrange their elements into a sequence that depends on the values
of the elements themselves, rather than the sequence in which we inserted them. Moreover,
associative containers exploit this ordering to let us locate particular elements much more
quickly than do the sequential containers, without our having to keep the container ordered by
ourselves.
	关联容器有自动排序功能，能够提供高效的 look-up 操作。
2. 关联容器通过 key(关键字) 进行高效的查找操作。
3. 一般的关联数据结构使用 key-value pairs。
4. When we put a particular key-value pair into the data structure, that key will
continue to be associated with the same value until we delete the pair. Such a data structure
is called an associative array(关联数组）。如 map 。

5. map<string, int> counters：That element is
value-initialized, which, for simple types such as int , is equivalent to setting the value to zero.
第一次值为0。
	counters[s]：其中 s 为 key，获取的是key关联的数据，在这里是 int。
6. The map container lets us do so by using a companion library type called *pair* .
7. A pair is a simple data structure that holds two elements, which are named first and second .
Each element in a map is really a pair , with a first member that contains the key and a
second member that contains the associated value. When we dereference a map iterator, we
obtain a value that is of the pair type associated with the map .
map中存放的实际是一系列的 pair 类型，当我们对map中的迭代器截解引用时，实际上获得的是 pair类型的数据。
8. key 总是 const 的。如对 map<string, int> 的迭代器解引用，实际上得到的是 pair<const string, int>。


