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
- (*iter).name // dereference 解引用，访问迭代器所在位置的元素
- iter->name // 迭代器可以作为指向元素的指针

7. 有些容器(container)支持 random-access indexing，有些容器不支持 random-access indexing。
即 students.begin() + i，其中i为整数，则有些容器支持 + 操作，则有些容器不支持 + 操作，只支持顺序访问。



