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

#ch8 generic function
1. generic function ，模板函数，只有在使用的时候才知道参数类型。
2. C++提供 generic function 特性的方法是使用 模板函数(template function)。
模板背后的关键思想是，不同类型的对象可能仍然具有共同的行为。
3. We do know the types when we use a template,
and that knowledge is available when we compile and link our programs。

4. Because different kinds of iterators offer different kinds of operations, it is important to
understand the requirements that various algorithms place on the iterators that they use, and
the operations that various kinds of iterators support.
5. The library defines five iterator categories, each one of which corresponds to a specific
collection of iterator operations. 每一种迭代器类型都有它相关的一组迭代器操作。
the iterator categories give us a way to understand
which containers can use which algorithms.

## 5种迭代器类型
1. If a type provides all of these operations, we call it an input iterator. Every container iterator
that we've seen supports all these operations, so they are all input iterators.
输入迭代器：支持 ++、==、!=、*、-> 等操作。每一个容器的迭代器都支持这些操作，所以它们都是输入迭代器。
Input iterators can be used only for reading elements of a sequence.(Sequential read-only access)。

2. output iterator(输出迭代器，只写）: Sequential write-only access。
All the standard containers provide iterators that meet these requirements, as does
back_inserter。
The iterator generated by back_inserter is an output iterator, so programs that use it must
obey the "write-once" requirement.
back_inserter 产生输出迭代器。

3. forward iterator(前向迭代器，可读可写, Sequential read-write access），支持以下操作：
	*it (for both reading and writing)
	++it and it++ (but not --it or it—-)
	it == j and it != j (where j has the same type as it)
	it->member (as a synonym for (*it).member)

	All the standard-library containers meet the forward-iterator requirements.

4. bidirectional iterator(双向迭代器，Reversible access)，不仅支持前向迭代器的操作，还支持 -- 操作。
	If a type meets all the requirements of a forward iterator, and also supports -- (both prefix and
postfix), we call it a bidirectional iterator.
	The standard-library container classes all support bidirectional iterators.

5. random-access iterators，支持以下操作：
	p + n, p - n, and n + p // p和q是迭代器，n是整数
	p - q
	p[n] (equivalent to *(p + n))
	p < q, p > q, p <= q, and p >= q
	不支持 == 和 != ？

	vector 和 string、sort函数支持 random-access iterators。但是 list 不支持，它只支持双向迭代器。


#ch9 Defining new types
1. C++支持2种数据类型
- build-in
- class types

2. 在头文件中一般使用完全限定名（the fully qualified names），如 std::vector，std::string，而不使用using声明，
如 using std::string 等。
3. 在 member function 后面添加 const 关键字，表示该函数不改变该 object 的任何的 member data。
const member function(const 成员函数）：Member functions that are const may not change the internal state of the object on
which they are executing。

4. cannot call non const functions on const objects。在const对象中不能调用非const函数。因为非const函数能够
改变对象的内部状态，而 const对象 被保护，只允许访问，不允许修改内部状态。
Only const member functions may be called for const objects.

5. struct 成员默认是 public；class 成员默认是 private。
6. Constructors（构造函数） are special member functions that define how objects are initialized.
7. The constructor that takes no arguments is known as the default constructor（默认构造函数）。
8. Between the : and the { is a sequence of
constructor initializers（初始化列表）, which tell the compiler to initialize the given members with the
values that appear between the corresponding parentheses.

std::string、std::vector会自动初始化。所以如果类中有 std::string，std::vector，则可不必对这些成员进行初始化。

9. When we create a new class object, several steps happen in sequence:
- 1) The implementation allocates memory to hold the object.
- 2) It initializes the object, as directed by the constructor's initializer list.
- 3) It executes the constructor body.

10. 重点理解以下这段话（关于构造函数初始化和赋值操作）：
The implementation initializes every data member of every object, regardless of whether the
constructor initializer list mentions those members. The constructor body may change these
initial values subsequently, but the initialization happens before the constructor body begins
execution. It is usually better to give a member an initial value explicitly, rather than assigning
to it in the body of the constructor. By initializing rather than assigning a value, we avoid doing
the same work twice.
不管构造函数的初始化列表有没有显式的初始化数据成员，编译器都会将其数据成员进行初始化。初始化发生在构造函数的函数体
执行之前，在构造函数的函数体的操作是赋值操作，我们应该显式的初始化我们的数据成员，即在初始化列表中进行初始化。

初始化的一些规则：
- If an object is of a class type that defines one or more constructors, then the
appropriate constructor completely controls initialization of the objects of that class.
- If an object is of built-in type, then value-initializing it sets it to zero, and default-
initializing it gives it an undefined value.
- Otherwise, the object can be only of a class type that does not define any
constructors. In that case, value- or default-initializing the object value- or
default-initializes each of its data members. This initialization process will be recursive
if any of the data members is of a class type with its own constructor.

We said that constructors exist to ensure that objects are created with their data members in a
sensible state. In general, this design goal means that every constructor should initialize every
data member. The need to give members a value is especially critical for members of built-in
type. If the constructor fails to initialize such members, objects declared at local scope will be
initialized with garbage, which is almost never correct.
构造函数的存在就是为了确保数据成员有一个合理的值或状态。每一个构造函数应该初始化每一个数据成员，对于
内置的类型来说，内置类型的初始化更为重要。

Data members that are not explicitly initialized are implicitly initialized.
未显式初始化的数据成员将被隐式初始化。

The order in which members are initialized is determined by the order of declaration in the
class, so care must be taken when using one class member to initialize another. It is safer
practice to avoid such interdependence by assigning values to these members inside the
constructor body and not initializing them in the constructor initializer.
成员初始化的顺序由类中的声明顺序决定，因此在使用一个类成员初始化另一个成员时必须小心。 通过在构造函数体内为这些成员赋值并且不在构造函数初始值设定项中初始化它来避免这种相互依赖是更安全的做法。


#ch10 
1. A pointer is a kind of random-access iterator that is essential for accessing elements of arrays, and has other uses as well.指针是一种随机访问迭代器，对于访问数组元素至关重要，并且还有其他用途。

2. A pointer is a value that represents the address of an object. Every distinct object has a unique
address, which denotes the part of the computer's memory that contains the object.

3. 函数指针
	int next(int n) { return n + 1; }
	fp = &next 等价于 fp = next
	i = (*fp)(i) 等价于 i = fp(i)

	double analysis(const vector<Student_info> &);
	double (*analysis)(const vector<Student_info> &); // analysis为函数指针
	typedef double (*analysis_fp)(const vector<Student_info> &) // analysis_fp 为函数指针类型，可用它来定义函数指针变量，
		如 analysis_fp fp; fp = analysis;

	函数指针一般用来做函数的参数，用于实现回调；也可以用来做返回值，但是很少见。
4. <cstddef> --> size_t类型、ptrdiff_t类型
5. A string literal（字符串常量） is really an array of const char with one more element than the number of characters in
the literal. That extra character is a null character (i.e., '\0' ) that the compiler automatically
appends to the rest of the characters.
	const char hello[] = { 'H', 'e', 'l', 'l', 'o', '\0' };
	he variable and the literal are two distinct objects and, therefore, have different
addresses.字符串常量和变量是两个不同的对象，各自有自己的地址。
6. <cstring> --> strlen() 计算以 '\0' 结尾的字符串的长度，不包括 '\0' 字符。
7. T× p = new T[n]; 
	vector<T> v(p, p + n);
	delete[] p;


#ch10 control copying/assigning/destorying class objects（重点理解构造函数和拷贝构造函数即赋值操作符）
1. explicit 关键字
	Now let's understand a bit about the use of explicit . This keyword makes sense only in the definition of a constructor that
takes a single argument. When we say that a constructor is explicit, we're saying that the compiler will use the constructor
only in contexts in which the user expressly invokes the constructor, and not otherwise:
	Vec<int> vi(100); // ok, explicitly construct the Vec from an int
	Vec<int> vi = 100; // error: implicitly construct the Vec (§11.3.3/199) and copy it to vi
	现在让我们了解一下使用explicit。 此关键字仅在采用单个参数的构造函数的定义中才有意义。 当我们说构造函数是显式的时，我们说编译器将仅在用户明确调用构造函数的上下文中使用构造函数，否则：

2. 两个相同类型的指针相减，返回这两个指针之间的元素的个数。

3. 拷贝构造函数（copy constructor）
Both explicit and implicit copies are controlled by a special constructor called the copy constructor.
	vector<int> vi;
	double d;
	d = median(vi); // copy vi into the parameter in median

	string line;
	vector<string> words = split(line); // copy the return from split into words

	vector<Student_info> vs;
	vector<Student_info> v2 = vs; // copy vs into v2, explicit

	隐式的发生 拷贝构造函数的情形：
	- Passing an object by value to a function。
	- returning an object by value from a function。


	深拷贝：如果有指针，则会在拷贝的同时重新分配内存。
	浅拷贝：仅仅拷贝数据，如果有指针，则两个对象指向同一块内存。

4. this关键字
The this keyword is valid only inside a member
function, where it denotes a pointer to the object on which the member function is operation.

5. When we use = to give an initial value to a variable, we are invoking the copy
constructor. When we use it in an assignment expression, we're calling operator= . Class authors must be attuned to the
difference in order to implement the right semantics.（ = 作为拷贝构造函数和 赋值操作符的区别 )

6. 初始化发生在：
- In variable declarations. 变量声明的时候
- For function parameters on entry to a function。
- For the return value of a function on return from the function
- In constructor initializers。 在初始化列表中。
7. 赋值运算发生在：
Assignment happens only when using the = operator in an expression.
8. rule of three: If your class needs a destructor, it probably needs a copy constructor and an
assignment operator too.
9. If we use new , it always initializes every element
of a T array by using T::T() . new 会调用构造函数初始化新创建的对象。

10. new 运算符为我们做了蛮多事，包括 申请内存和使用所分配类型的默认构造函数进行初始化
The new operator does too much
for our purposes: It both allocates and initializes memory. When used to allocate an array of
type T , it needs the default constructor for T .

If we use new , it always initializes every element of a T array by using T::T() .
即 使用 new 分配内存时，它会做两件事，一件事就是分配内存，另一件事就是使用类型的默认构造函数对每一个元素进行初始化。

一般使用 new 分配内存时，我们都会在分配内存后对每个元素进行初始化，而 new操作符又会进行初始化，这样的话，使用 new 运算符再加上
后面的人工初始化，这样就对一个元素进行了2次初始化，开销是蛮大的。
对于 STL 标准库中的容器，为了避免两次的初始化，一般都不使用 new 和 delete 来进行内存的管理，而是使用 标准库<memory> 中提供的
allocator<T> 类来进行内存的管理。

11. 更为灵活的内存管理策略：<memory> 中的 allocator<T> 类型：
The <memory> header provides a class, called allocator<T> , that allocates a block of
uninitialized memory that is intended to contain objects of type T, and returns a pointer to the
initial element of that memory.

The library alsoprovides a way to construct objects in that memory, and to destroy the objects again—all
without deallocating the memory itself.It is up to the programmer using the allocator class to
keep track of which space holds constructed objects and which space is still uninitialized.

The construct and destroy members construct or destroy a single object（单个对象）in this uninitialized space.
