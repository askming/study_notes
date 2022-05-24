# 廖雪峰python教程笔记

## 循环
可以利用`break`提前终止循环或者`continue`跳过当前循环：

```python
# break
n = 1
while n <= 100:
   if n > 10: # 当n = 11时，条件满足，执行break语句
      break # break语句会结束当前循环
      print(n)
      n = n + 1
print('END')

# continue
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)
```

## 使用dict和set
- dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是**dict的key必须是不可变对象**。 这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。**这个通过key计算位置的算法称为哈希算法（Hash）**。
- list是可变的，不能作为key。

- set和dict类似，也是一组key的集合，但不存储value。set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作
- set和dict的唯一区别仅在于没有存储对应的value，但是，set的原理和dict一样，所以，同样不可以放入可变对象，因为无法判断两个可变对象是否相等，也就无法保证set内部“不会有重复元素”。

## 函数
- 函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：
    ```python
    >>> a = abs # 变量a指向abs函数
    >>> a(-1) # 所以也可以通过a调用abs函数
    1
    ```
- 函数执行完毕也没有return语句时，自动return None。
- 函数可以同时返回多个值，但其实就是一个tuple。

### 空函数

- 如果想定义一个什么事也不做的空函数，可以用pass语句：
    ``` python
    def nop():
        pass
    ```
    
    `pass` 可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个`pass`，让代码能运行起来。

### 函数的参数
* 位置参数
* 默认参数
    - 设置默认参数时，有几点要注意：
          一是必选参数在前，默认参数在后，否则Python的解释器会报错（思考一下为什么默认参数不能放在必选参数前面；
          二是如何设置默认参数。当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。
    - 使用默认参数最大的好处是能降低调用函数的难度。
    - 定义默认参数要牢记一点：**默认参数必须指向不变对象！**
    - 为什么要设计`str`、`None`这样的不变对象呢？因为不变对象一旦创建，对象内部的数据就不能修改，这样就减少了由于修改数据导致的错误。此外，由于对象不变，多任务环境下同时读取对象不需要加锁，同时读一点问题都没有。**我们在编写程序时，如果可以设计一个不变对象，那就尽量设计成不变对象**。
* 可变参数
    - 定义函数的时候在参数名前加星号（`*`）
    ```python
    def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
    ```
    - Python允许你在list或tuple前面加一个`*`号，把list或tuple的元素变成可变参数传进去：
    ```python
    >>> nums = [1, 2, 3]
    >>> calc(*nums)
    14
    ```
* 关键字参数
    - 关键字参数允许你传入0个或任意个含参数名的参数，**这些关键字参数在函数内部自动组装为一个dict**：
        ```python
        def person(name, age, **kw):
        print('name:', name, 'age:', age, 'other:', kw)

        >>> person('Adam', 45, gender='M', job='Engineer')
        name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

        >>> extra = {'city': 'Beijing', 'job': 'Engineer'}
        >>> person('Jack', 24, **extra)
        name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
        ```
    - 和可变参数类似，也可以先组装出一个dict，然后，把该dict转换为关键字参数传进去：
        ```python
        >>> extra = {'city': 'Beijing', 'job': 'Engineer'}
        >>> person('Jack', 24, city=extra['city'], job=extra['job'])
        name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
        ```

        或者更简洁地：
        ```python
        >>> person('Jack', 24, **extra)
        name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
        ```
        
        `**extra`表示把`extra`这个`dict`的所有key-value用关键字参数传入到函数的`**k`w参数，`kw`将获得一个dict，注意kw获得的dict是`extra`的一份拷贝，对`kw`的改动不会影响到函数外的`extra`。

* 命名关键字参数
    - 如果要限制关键字参数的名字，就可以用命名关键字参数
        ```python
        def person(name, age, *, city, job):
        print(name, age, city, job)
        ```
    
        和关键字参数`**kw`不同，命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数。

    - **如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了**
    - 命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错

* 参数组合
    - 在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是请注意，**参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数**。
    - 对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。`*args` 接收的是一个tuple; `**kw` 接收的是一个dict
        ```python
        def f1(a, b, c=0, *args, **kw):
        print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

        def f2(a, b, c=0, *, d, **kw):
        print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)

        >>> args = (1, 2, 3, 4)
        >>> kw = {'d': 99, 'x': '#'}
        >>> f1(*args, **kw)
        a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
        >>> args = (1, 2, 3)
        >>> kw = {'d': 88, 'x': '#'}
        >>> f2(*args, **kw)
        a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
        ```

* 小结
    - 默认参数一定要用不可变对象，如果是可变对象，程序运行时会有逻辑错误！
    - 命名的关键字参数是为了限制调用者可以传入的参数名，同时可以提供默认值。
    - 定义命名的关键字参数在没有可变参数的情况下不要忘了写分隔符`*`，否则定义的将是位置参数。

### 递归函数(recursive functions)
- 如果一个函数在内部调用自身本身，这个函数就是递归函数。
- 理论上，所有的递归函数都可以写成循环的方式，但循环的逻辑不如递归清晰。
- 使用递归函数需要注意防止栈溢出。在计算机中，函数调用是通过 **栈(stack)** 这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出。(--> stack overflow) 
- 解决递归调用栈溢出的方法是通过 **尾递归** 优化，事实上尾递归和循环的效果是一样的，所以，把循环看成是一种特殊的尾递归函数也是可以的
- 尾递归是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

- 例子：[汉诺塔][1]的移动

    *逻辑：初始从A通过B为过渡移动到终点C，然后以B为起点A为过渡转移至终点C。每一步（A --> C 或 B --> C）先转移当前最大的那个到C，通过A,B互相作为过渡柱，最终实现全部转移。*
    ```python
    def move(n, a, b, c):
        if n == 1:
            print('move', a, '-->', c)
        else:
            move(n-1, a, c, b) # 除去最下面一个以外，先把A柱上的移到B柱 （通过C为过渡， 此时B为终点柱）
            move(1, a, b, c) # 把A柱最大一个放到目标终点C柱上
            move(n-1, b, a, c) # 把A上的转移到B以后的新目标：把B柱上的通过A柱为过渡移到C柱

    # move(3, 'A', 'B', 'C')
    # A --> C
    # A --> B
    # C --> B
    # A --> C
    # B --> A
    # B --> C
    # A --> C
    ```
## 高级特性
### 切片
- 切片（slice）：取一个list或tuple的部分元素是非常常见的操作。
- tuple也是一种list，唯一区别是tuple不可变。因此，tuple也可以用切片操作，只是操作的结果仍是tuple
- 字符串`'xxx'`也可以看成是一种list，每个元素就是一个字符。因此，字符串也可以用切片操作，只是操作结果仍是字符串。 在很多编程语言中，针对字符串提供了很多各种截取函数（例如，substring），其实目的就是对字符串切片。Python没有针对字符串的截取函数，只需要切片一个操作就可以完成，非常简单。

### 迭代
- 定义：如果给定一个list或tuple，我们可以通过for循环来遍历这个list或tuple，这种遍历我们称为 **迭代（Iteration）**。
- 当我们使用`for`循环时，只要作用于一个可迭代对象，`for`循环就可以正常运行，而我们不太关心该对象究竟是list还是其他数据类型。
- 那么，如何判断一个对象是可迭代对象呢？方法是通过collections模块的Iterable类型判断：
    ```python
    >>> from collections import Iterable
    >>> isinstance([1,2,3], Iterable) # list是否可迭代
    True
    >>> isinstance(123, Iterable) # 整数是否可迭代
    False
    ```
- Python内置的`enumerate`函数可以把一个list变成索引-元素对，这样就可以在`for`循环中同时迭代索引和元素本身：
    ```python
    >>> for i, value in enumerate(['A', 'B', 'C']):
    ...     print(i, value)
    ...
    0 A
    1 B
    2 C
    ```

### 列表生成式(list comprehension)
- 列表生成式则可以用一行语句代替循环生成list：
    ```python
    >>> [x * x for x in range(1, 11)]
    [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    ```
- 还可以使用两层循环，可以生成全排列(三层和三层以上的循环就很少用到了)：
    ```python
    >>> [m + n for m in 'ABC' for n in 'XYZ']
    ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
    ```

### 生成器(generator)
- 目的：不一次性直接生成list的全部元素（避免因为list过大而占用很大的存储空间或者不需要马上使用list后面的元素），取而代之的是一边循环一边生成新的元素。
- 要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的`[]`改成`()`，就创建了一个generator：
    ```python
    >>> g = (x * x for x in range(10))
    >>> g
    <generator object <genexpr> at 0x1022ef630>
    ```
- 然后，可以通过next()函数获得generator的下一个返回值：
    ```python
    >>> next(g)
    0
    >>> next(g)
    1
    >>> next(g)
    .
    .
    .
    >>> next(g)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    StopIteration
    ```
- 正确的方法是使用for循环，因为`generator`也是可迭代对象. 我们创建了一个`generator`后，基本上永远不会调用`next()`，而是通过`for`循环来迭代它，并且不需要关心`StopIteration`的错误。

- 通过定义函数生成generator：只需要把`print()`改为`yield` 就可以了
    ```python
    def fib(max):
        n, a, b = 0, 0, 1
        while n < max:
            yield b
            a, b = b, a + b
            n = n + 1
        return 'done'
    ```

- `generator`和函数的执行流程不一样。函数是顺序执行，遇到`return`语句或者最后一行函数语句就返回。而变成`generator`的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的`yield`语句处继续执行。

- 用`for`循环调用`generator`时，发现拿不到`generator`的`return`语句的返回值。如果想要拿到返回值，必须捕获`StopIteration`错误，返回值包含在`StopIteration`的`value`中：
    ```python
    >>> g = fib(6)
    >>> while True:
    ...     try:
    ...         x = next(g)
    ...         print('g:', x)
    ...     except StopIteration as e:
    ...         print('Generator return value:', e.value)
    ...         break
    ...
    g: 1
    g: 1
    g: 2
    g: 3
    g: 5
    g: 8
    Generator return value: done
    ```
- 练习：生成杨辉三角
    ```python
    def triangles():
        lst = [1]
        while True:
            yield lst
            lst = [1] + [lst[i] + lst[i+1] for i in range(len(lst)-1)] + [1]
        return 'done'

    n = 0
    for t in triangles():
        print(t)
        n = n + 1
        if n == 10:
            break
    [1]
    [1, 1]
    [1, 2, 1]
    [1, 3, 3, 1]
    [1, 4, 6, 4, 1]
    [1, 5, 10, 10, 5, 1]
    [1, 6, 15, 20, 15, 6, 1]
    [1, 7, 21, 35, 35, 21, 7, 1]
    [1, 8, 28, 56, 70, 56, 28, 8, 1]
    [1, 9, 36, 84, 126, 126, 84, 36, 9, 1]
    ```

### 迭代器
- 定义：可以被`next()`函数调用并不断返回下一个值的对象称为迭代器：`Iterator`。
- 可以使用`isinstance()`判断一个对象是否是`Iterator`对象：
    ```python
    >>> from collections import Iterator
    >>> isinstance((x for x in range(10)), Iterator)
    True
    ```
- 把`list`、`dict`、`str`等`Iterable`变成`Iterator`可以使用`iter()`函数：
    ```python
    >>> isinstance(iter([]), Iterator)
    True
    ```

- Python的`Iterator`对象表示的是一个数据流，`Iterator`对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出`StopIteration`错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过`next()`函数实现按需计算下一个数据，所以`Iterator`的计算是惰性的，只有在需要返回下一个数据时它才会计算。`Iterator`甚至可以表示一个无限大的数据流，例如全体自然数。而使用`list`是永远不可能存储全体自然数的。


## 函数式编程
- 函数是Python内建支持的一种封装，我们通过把大段代码拆成函数，通过一层一层的函数调用，就可以把复杂任务分解成简单的任务，这种分解可以称之为 **面向过程**的程序设计。函数就是面向过程的程序设计的基本单元。

- 函数式编程就是一种抽象程度很高的编程范式，纯粹的函数式编程语言编写的函数*没有变量*，因此，任意一个函数，只要输入是确定的，输出就是确定的，这种纯函数我们称之为 **没有副作用**。而*允许使用变量*的程序设计语言，由于函数内部的变量状态不确定，同样的输入，可能得到不同的输出，因此，这种函数是 **有副作用的**。

- 函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！

- Python对函数式编程提供部分支持。由于Python允许使用变量，因此，Python不是纯函数式编程语言。

### 高阶函数（Higher-order function）
* 变量可以指向函数
* 函数名也是变量
    - 函数名其实就是指向函数的变量！
        ```python
        >>> abs = 10
        >>> abs(-10)
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        TypeError: 'int' object is not callable
        ```
    - 由于`abs`函数实际上是定义在`import builtins`模块中的，所以要让修改`abs`变量的指向在其它模块也生效，要用`import builtins; builtins.abs = 10`。

* 传入函数
    
    - 既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为 **高阶函数**。函数式编程就是指这种高度抽象的编程范式。

#### map/reduce
- `map()`函数接收两个参数，一个是函数，一个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。`map()`作为高阶函数，事实上它把运算规则抽象了。
- `reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是：
    ```python
    reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
    ```
- 例子， 如果要把序列[1, 3, 5, 7, 9]变换成整数13579，reduce就可以派上用场：
    ```python
    >>> from functools import reduce
    >>> def fn(x, y):
    ...     return x * 10 + y
    ...
    >>> reduce(fn, [1, 3, 5, 7, 9])
    13579
    ```
- 练习：利用`map()`函数，把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。
    ```python
    def normalize(name):
        return(name[0].upper() + name[1:].lower())

    L1 = ['adam', 'LISA', 'barT']
    L2 = list(map(normalize, L1))
    print(L2)
    >>> ['Adam', 'Lisa', 'Bart']
    ```
- 练习：利用`reduce()`求积：
    ```python
    from functools import reduce
    def prod(L):
    return reduce(lambda x, y: x*y, L)
    print('3 * 5 * 7 * 9 =', prod([3, 5, 7, 9]))
    3 * 5 * 7 * 9 = 945
    ```
- 练习：利用`map`和`reduce`编写一个`str2float`函数，把字符串`'123.456'`转换成浮点数`123.456`：
    ```python
    from functools import reduce
    def str2float(s):
        nums = map(lambda ch: CHAR_TO_FLOAT[ch], s)
        point = 0
        def to_float(f, n):
            nonlocal point
            if f == -1:
                point = 1
                return 0
            if n == -1:
                point = 1
                return f
            if point == 0:
                return f * 10 + n
            else:
                point = point * 10
                return f + n / point
        return reduce(to_float, nums)
    ```
- [google 关于MapReduce的论文][2]

#### filter
- 和`map()`类似，`filter()`也接收一个函数和一个序列。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。
- 注意到`filter()`函数返回的是一个`Iterator`，也就是一个惰性序列，所以要强迫`filter()`完成计算结果，需要用`list()`函数获得所有结果并返回`list`。

- 计算素数的一个方法是[埃氏筛法][3]，它的算法理解起来非常简单：
    首先，列出从2开始的所有自然数，构造一个序列：

    2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...

    取序列的第一个数2，它一定是素数，然后用2把序列的2的倍数筛掉：

    3, ~4~, 5, ~6~, 7, ~8~, 9, ~10~, 11, ~12~, 13, ~14~, 15, ~16~, 17, ~18~, 19, ~20~, ...

    取新序列的第一个数3，它一定是素数，然后用3把序列的3的倍数筛掉：

    5, 7, ~9~, 11, 13, ~15~, 17, 19, ...

    取新序列的第一个数5，然后用5把序列的5的倍数筛掉：

    7, 11, 13, 17, 18, 19, ...

    不断筛下去，就可以得到所有的素数。

    ```python
    #先构造一个从3开始的奇数序列：
    def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n

    #定义一个筛选函数：
    def _not_divisible(n):
    return lambda x: x % n > 0

    #最后，定义一个生成器，不断返回下一个素数：
    def primes():
    yield 2
    it = _odd_iter() # 初始序列
    while True:
        n = next(it) # 返回序列的第一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列

    # primes()也是一个无限序列，所以调用时需要设置一个退出循环的条件：
    # 打印1000以内的素数:
    for n in primes():
        if n < 1000:
            print(n)
        else:
            break
    ```
- 练习：回数是指从左向右读和从右向左读都是一样的数，例如`12321`，`909`。请利用`filter()`滤掉非回数。
    ```python
    def is_palindrome(n):
    n_str = str(n)
    return(n_str[:] == n_str[-1::-1])

    output = filter(is_palindrome, range(1, 1000))
    print(list(output))
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 22, 33, 44, 55, 66, 77, 88, 99, 101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 202, 212, 222, 232, 242, 252, 262, 272, 282, 292, 303, 313, 323, 333, 343, 353, 363, 373, 383, 393, 404, 414, 424, 434, 444, 454, 464, 474, 484, 494, 505, 515, 525, 535, 545, 555, 565, 575, 585, 595, 606, 616, 626, 636, 646, 656, 666, 676, 686, 696, 707, 717, 727, 737, 747, 757, 767, 777, 787, 797, 808, 818, 828, 838, 848, 858, 868, 878, 888, 898, 909, 919, 929, 939, 949, 959, 969, 979, 989, 999]
    ```

#### sorted
- Python内置的`sorted()`函数就可以对`list`进行排序。`sorted()`函数也是一个高阶函数，它还可以接收一个`key`函数来实现自定义的排序，例如按绝对值大小排序：
    ```python
    >>> sorted([36, 5, -12, 9, -21], key=abs)
    [5, 9, -12, -21, 36]
    ```
- 对字符串排序，是按照ASCII的大小比较的。 若想排序应该忽略大小写，按照字母序排序， 只要我们能用一个key函数把字符串映射为忽略大小写排序即可：
    ```python
    >>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
    ['about', 'bob', 'Credit', 'Zoo']
    ```
- 要进行反向排序，不必改动`key`函数，可以传入第三个参数`reverse=True`：
    ```python
    >>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
    ['Zoo', 'Credit', 'bob', 'about']
    ```
- 练习：假设我们用一组tuple表示学生名字和成绩 `L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]`, 请用`sorted()`对上述列表分别按名字/成绩排序：
    ```python
    from operator import itemgetter
    L2 = sorted(L, key = itemgetter(0))
    L3 = sorted(L, key = itemgetter(1), reverse = True)
    ```

### 返回函数
#### 闭包(closure)
- 内部函数可以引用外部函数的参数和局部变量，在外部函数返回内部函数时，相关参数和变量都保存在返回的函数中，这种称为 **“闭包”(closure)**，closure拥有极大的威力。
- 返回的函数并没有立刻执行，而是直到调用了被返回的函数才执行。
    - 例子
        ```python
        def count():
        fs = []
        for i in range(1, 4):
            def f():
                 return i*i
            fs.append(f)
        return fs

        f1, f2, f3 = count()
        >>> f1()
        9
        >>> f2()
        9
        >>> f3()
        9
        ```
    原因就在于返回的函数引用了变量`i`，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量`i`已经变成了`3`，因此最终结果为`9`。
- 返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量。
- 如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：
    ```python
    def count():
        def f(j):
            def g():
                return j*j
            return g
        fs = []
        for i in range(1, 4):
            fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
        return fs
    ```

### 匿名函数
- 当我们在传入函数时，有些时候，不需要显式地定义函数，直接传入 **匿名函数**更方便。
- 关键字`lambda`表示匿名函数，冒号前面的`x`表示函数参数。
- 匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。


### 装饰器(Decorator)
- 由于函数也是一个对象，而且函数对象可以被赋值给变量，所以，通过变量也能调用该函数。函数对象有一个`__name__`属性，可以拿到函数的名字：
    ```python
    >>> def now():
    ...     print('2015-3-25')
    ...
    >>> f = now
    >>> f.__name__
    'now'
    ```

- 在代码运行期间动态增加功能的方式，称之为 **“装饰器”（Decorator）**
- 本质上，decorator就是一个返回函数的高阶函数:
    ```python
    def log(func):
        def wrapper(*args, **kw):
            print('call %s():' % func.__name__)
            return func(*args, **kw)
        return wrapper
    ```
    
    我们要借助Python的`@`语法，把decorator置于函数的定义处：
    ```python
    @log
    def now():
        print('2015-3-25')
    ```

    把`@log`放到`now()`函数的定义处，相当于执行了语句：
```python 
now = log(now)
```

- 由于`log()`是一个decorator，返回一个函数，所以，原来的`now()`函数仍然存在，只是现在同名的now变量指向了新的函数，于是调用`now()`将执行新函数，即在`log()`函数中返回的`wrapper()`函数。`wrapper()`函数的参数定义是`(*args, **kw)`，因此，`wrapper()`函数可以接受任意参数的调用。

- 如果decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数，写出来会更复杂。比如，要自定义log的文本：
    ```python
    def log(text):
        def decorator(func):
            def wrapper(*args, **kw):
                print('%s %s():' % (text, func.__name__))
                return func(*args, **kw)
            return wrapper
        return decorator

    @log('execute')
    def now():
        print('2015-3-25')
    ```

    和两层嵌套的decorator相比，3层嵌套的效果是这样的：

    ```python 
    >>> now = log('execute')(now)
    ```
- 函数经过decorator以后原来函数的`__name__`属性会变成wrapper函数的`__name__`属性，需要把原始函数的`__name__`等属性复制到`wrapper()`函数中，否则，有些依赖函数签名的代码执行就会出错。Python内置的`functools.wraps`就是干这个事的

- 练习: 请编写一个`decorator`，能在函数调用的前后打印出`'begin call'`和`'end call'`的日志。
    ```python
    import functools
    def log(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('begin call %s():' % func.__name__)
            out = func(*args, **kw)
            print('end call %s.' % func.__name__)
            return out
        return wrapper

    @log
    def today():
        print('2017-09-16')

    today()

    >>> begin call today():
    2017-09-16
    end call today.
    ```

### 偏函数(partial function)
- 把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。
- `functools.partial`可以帮助我们创建一个偏函数：
    ```python
    >>> import functools
    >>> int2 = functools.partial(int, base=2)
    ```
- 创建偏函数时，实际上可以接收函数对象、`*args`和`**kw`这3个参数，当传入：
    ```python
    int2 = functools.partial(int, base=2)
    ```
    相当于：
    ```python
    kw = { 'base': 2 }
    int('10010', **kw)
    ```
- 当传入：`max2 = functools.partial(max, 10)` 实际上会把10作为`*args`的一部分自动加到左边，也就是：`max2(5, 6, 7)` 相当于：`args = (10, 5, 6, 7)`, `max(*args)`。

## 模块(Module)
- 为了便于管理众多的函数代码，提高代码的可维护性。在Python中，一个.py文件就称之为一个 **模块（Module）**。
- 使用模块还可以避免函数名和变量名冲突。相同名字的函数和变量完全可以分别存在不同的模块中，因此，我们自己在编写模块时，不必考虑名字会与其他模块冲突。但是也要注意，尽量不要与内置函数名字冲突。
- 如果不同的人编写的模块名相同怎么办？为了避免模块名冲突，Python又引入了按目录来组织模块的方法，称为 **包（Package）**。即，把多个模块放入一个包内，只要顶层的包不与别人冲突，那所有内部的模块都不会与人冲突。
- 请注意，每一个包目录下面都会有一个`__init__.py`的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。`__init__.py`可以是空文件，也可以有Python代码，因为`__init__.py`本身就是一个模块，而它的模块名就是该包的名字。
- 包内还可以有更低一级的目录，内含若干的模块，以此类推。 

### 使用模块
- 任何模块代码的第一个字符串都被视为模块的文档注释；
- 例子：
    ```python
    #!/usr/bin/env python3 #让文件可以直接在Unix/Linux/Mac上运行
    # -*- coding: utf-8 -*- #表示.py文件本身使用标准UTF-8编码；

    ' a test module '

    __author__ = 'Michael Liao'

    import sys

    def test():
        args = sys.argv # sys模块有一个argv变量，用list存储了命令行的所有参数
        if len(args)==1:
            print('Hello, world!')
        elif len(args)==2:
            print('Hello, %s!' % args[1])
        else:
            print('Too many arguments!')

    if __name__=='__main__': # 在命令行运行hello模块文件时，Python解释器把一个特殊变量__name__置为__main__； 让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。
        test()
    ```

#### 作用域
- 在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我们希望给别人使用（public），有的函数和变量我们希望仅仅在模块内部使用（private）。在Python中，是通过`_`前缀来实现的。
- private函数和变量“不应该”被直接引用，而不是“不能”被直接引用，是因为Python并没有一种方法可以完全限制访问private函数或变量，但是，从编程习惯上不应该引用private函数或变量。
- 这是一种非常有用的代码封装和抽象的方法，即：外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public。

### 安装第三方模块
#### 模块搜索路径
- 默认情况下，Python解释器会搜索当前目录、所有已安装的内置模块和第三方模块，搜索路径存放在`sys`模块的`path`变量中
    ```python
    >>> import sys
    >>> sys.path
    ```

- 添加自己的搜索目录
    ```python
    sys.path.append('\path') #只在运行时修改，运行结束后失效
    ```
    第二种方法是设置环境变量`PYTHONPATH`，该环境变量的内容会被自动添加到模块搜索路径中。


## 面向对象编程(Object Oriented Programming, OOP)
- OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。
- **面向过程的程序设计** 把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。
- **而面向对象的程序设计** 把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递
- 面向对象的设计思想是从自然界中来的，因为在自然界中，类（Class）和实例（Instance）的概念是很自然的。
- 数据封装、继承和多态是面向对象的三大特点

### 类和实例
- 类是抽象的模板，而实例是根据类创建出来的一个个具体的“对象”，每个对象都拥有相同的方法，但各自的数据可能不同。
    ```python
    class Student(object):
        pass
    ```
    类名通常是大写开头的单词，紧接着是(`object`)，表示该类是从哪个类继承下来的，通常，如果没有合适的继承类，就使用`object`类，这是所有类最终都会继承的类。
- 可以自由地给一个实例变量绑定属性，比如，给实例bart绑定一个`name`属性：
    ```python
    >>> bart = Student()
    >>> bart.name = 'Bart Simpson'
    >>> bart.name
    'Bart Simpson'
    ```
- 和静态语言不同，Python允许对实例变量绑定任何数据，也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同

- 由于类可以起到模板的作用，因此，可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。通过定义一个特殊的`__init__`方法，在创建实例的时候，就把`name`，`score`等属性绑上去：
    ```python
    class Student(object):
        def __init__(self, name, score):
            self.name = name
            self.score = score
    ```
    注意到`__init__`方法的第一个参数永远是`self`，表示创建的实例本身，因此，在`__init__`方法内部，就可以把各种属性绑定到`self`，因为`self`就指向创建的实例本身。有了`__init__`方法，在创建实例的时候，就不能传入空的参数了，必须传入与`__init__`方法匹配的参数，但`self`不需要传，Python解释器自己会把实例变量传进去。

- 和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量`self`，并且，调用时，不用传递该参数。除此之外，类的方法和普通函数没有什么区别，所以，你仍然可以用默认参数、可变参数、关键字参数和命名关键字参数。

#### 数据封装
- 直接在类的内部定义访问数据的函数，这样，就把“数据”给封装起来了。这些封装数据的函数是和类本身是关联起来的，我们称之为类的 **方法 (method)**。 要定义一个方法，除了第一个参数是`self`外，其他和普通函数一样。要调用一个方法，只需要在实例变量上直接调用，除了`self`不用传递，其他参数正常传入。


### 访问限制
- 如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线__，在Python中，实例的变量名如果以`__`开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问
- 如果又要允许外部修改私有变量的值，可以在类内部定义一个方法来完成，这样可以保证在赋新值的时候做一些必要的检查而避免被赋无效值。
- 需要注意的是，在Python中，变量名类似`__xxx__`的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以，不能用`__name__`、`__score__`这样的变量名。
- 有些时候，你会看到以一个下划线开头的实例变量名，比如`_name`，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。
- 双下划线开头的实例变量是不是一定不能从外部访问呢？其实也不是。不能直接访问`__name`是因为Python解释器对外把`__name`变量改成了`_Student__name`，所以，仍然可以通过`_Student__name`来访问`__name`变量。 不同版本的Python解释器可能会把`__name`改成不同的变量名。

### 继承和多态
- 在OOP程序设计中，当我们定义一个class的时候，可以从某个现有的class继承，新的class称为 **子类（Subclass）**，而被继承的class称为 **基类、父类或超类（Base class、Super class）**。
- 如何定义： `class subclass_name(base_class_name):`
- 继承有什么好处？最大的好处是子类获得了父类的全部功能。继承的第二个好处是只要对代码做一点改进而实现某个特定方法的“定制 (customization)”。
- 当子类和父类都存在相同的方法时，我们说，子类的方法覆盖了父类的方法，在代码运行的时候，总是会调用子类的方法。这样，我们就获得了继承的另一个好处：**多态**。
- 当我们定义一个class的时候，我们实际上就定义了一种数据类型。我们定义的数据类型和Python自带的数据类型，比如str、list、dict没什么两样, 可以用`isinstance()`来判断数据类型。在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类。但是，反过来就不行。
- 多态的好处就是： 对于一个变量，我们只需要知道它是类型，无需确切地知道它的子类型，就可以放心地调用某一多态方法，而具体调用的方法是作用在子类还是父类对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种子类时，只要确保多态方法编写正确，不用管原来的代码是如何调用的。这就是著名的“开闭”原则：

    - **对扩展开放**：允许新增子类；

    - **对修改封闭**：不需要修改依赖父类类型的方法函数。

- 继承还可以一级一级地继承下来，就好比从爷爷到爸爸、再到儿子这样的关系。而任何类，最终都可以追溯到根类object，这些继承关系看上去就像一颗倒着的树。比如如下的继承树

#### 静态语言 vs 动态语言
- 对于静态语言（例如Java）来说，如果需要传入某类型，则传入的对象必须是该类型或者它的子类，否则，将无法调用父类型下的方法。
- 对于Python这样的动态语言来说，则不一定需要传入父类型类型。我们只需要保证传入的对象有一个同名的方法就可以了。
- 这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。


### 获取对象信息
#### 使用`type()`
- `type()`返回对应的class类型
- 借助`types` 模块来判断一个对象是否是函数：
    ```python
    >>> import types
    >>> def fn():
    ...     pass
    ...
    >>> type(fn)==types.FunctionType
    True
    >>> type(abs)==types.BuiltinFunctionType
    True
    >>> type(lambda x: x)==types.LambdaType
    True
    >>> type((x for x in range(10)))==types.GeneratorType
    True
    ```

#### 使用`isinstance()`
- 更加便于了解class的继承关系
- 能用`type()`判断的基本类型也可以用`isinstance()`判断
- 还可以判断一个变量是否是某些类型中的一种
    ```python
    >>> isinstance([1, 2, 3], (list, tuple))
    True
    ```

#### 使用`dir()`
- 可以用来获取一个对象的所有属性和方法
- 类似`__xxx__`的属性和方法在Python中都是有特殊用途的，比如`__len__`方法返回长度。剩下的都是普通属性或方法。
- 仅仅把属性和方法列出来是不够的，配合`getattr()`、`setattr()`以及`hasattr()`，我们可以直接操作一个对象的状态。
- 如果试图获取不存在的属性，会抛出AttributeError的错误。 可以传入一个default参数，如果属性不存在，就返回默认值：
    ```python
    >>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
    404
    ```
- 通过类似的操作也可以获得对象的方法
- 一个正确的用法的例子如下：
    ```python
    def readImage(fp):
        if hasattr(fp, 'read'):
            return readData(fp)
    return None
    ```
    假设我们希望从文件流fp中读取图像，我们首先要判断该fp对象是否存在read方法，如果存在，则该对象是一个流，如果不存在，则无法读取。
    `hasattr()`就派上了用场。

### 实例属性和类属性
- 给实例绑定属性的方法是通过实例变量，或者通过self变量：
    ```python
    class Student(object):
        def __init__(self, name):
            self.name = name
    s = Student('Bob')
    ```
- 当我们定义了一个类属性后，这个属性虽然归类所有，但类的所有实例都可以访问到。实例属性高于类属性，当实例和类定义相同的属性时，实例属性将覆盖类属性。但是当你删除实例属性后，再使用相同的名称，访问到的将是类属性。


## 面向对象高级编程
数据封装、继承和多态只是面向对象程序设计中最基础的3个概念。在Python中，面向对象还有很多高级特性，允许我们写出非常强大的功能。例如：多重继承、定制类、元类等。


### 使用__slots__
- 动态绑定允许我们在程序运行的过程中动态给class加上功能，这在静态语言中很难实现。
- 但是，如果我们想要限制实例的属性怎么办？为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性：
    ```python
    class Student(object):
        __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
    ```
- 使用`__slots__`要注意，`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的。除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`。

### 使用@property
- 主要目的是为了更加方便地给实例赋值（ **通过属性赋值形式实现方法方式赋值过程**），同时又能进行一些必要的赋值检查（只能在方法中定义检查赋值的有效范围）。
- @property 相当于`getter`方法（即读取）， 而它会制动产生一个`setter`方法（即赋值）。
    ```python
    class Student(object):
        @property
        def score(self):
            return self._score
        @score.setter
        def score(self, value):
            if not isinstance(value, int):
                raise ValueError('score must be an integer!')
            if value < 0 or value > 100:
                raise ValueError('score must between 0 ~ 100!')
            self._score = value
    ```
- `@property`广泛应用在类的定义中，可以让调用者写出简短的代码，同时保证对参数进行必要的检查，这样，程序运行时就减少了出错的可能性。


### 多重继承
- 继承是面向对象编程的一个重要的方式，因为通过继承，子类就可以扩展父类的功能。
- 多重继承即子类可以同时继承多个父类

#### MixIn (mix inherit)
- MixIn的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个MixIn的功能，而不是设计多层次的复杂的继承关系。
    ```python
    class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
        pass
    ```

### 定制类
- 看到类似`__slots__`这种形如`__xxx__`的变量或者函数名就要注意，这些在Python中是有特殊用途的。

#### `__str__`
- 对应`print`方法: 只需要定义好`__str__()`方法，返回一个好看的字符串就可以了：
    ```python
    >>> class Student(object):
    ...     def __init__(self, name):
    ...         self.name = name
    ...     def __str__(self):
    ...         return 'Student object (name: %s)' % self.name
    ...
    >>> print(Student('Michael'))
    Student object (name: Michael)
    ```
- 若只用变量名而不用`print`直接显示变量调用的不是`__str__()`，而是`__repr__()`，两者的区别是`__str__()`返回用户看到的字符串，而`__repr__()`返回程序开发者看到的字符串，也就是说，`__repr__()`是为调试服务的。所以可以把`__str__` 赋值给`__repr__`: `__repr__ = __str__`

#### `__iter__`
- 如果一个类想被用于`for ... in`循环，类似`list`或`tuple`那样，就必须实现一个`__iter__()`方法，该方法返回一个迭代对象，然后，Python的`for`循环就会不断调用该迭代对象的`__next__()`方法拿到循环的下一个值，直到遇到`StopIteration`错误时退出循环。

#### `__getitem__`
- Fib实例虽然能作用于for循环，看起来和list有点像，但是，把它当成list来使用还是不行, 比如不能用index取其中的元素。


#### `__getattr__`
- 当输入的属性在默认属性中不存在，python调用`__getattr__`查找。任意调用不存在的属性都会返回`None`。也可以对不存在的属性自定义抛出`AttributeError`的错误。
    ```python
    class Student(object):
        def __getattr__(self, attr):
            if attr=='age':
                return lambda: 25
            raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
    ```

#### `__call__`
- 一个对象实例可以有自己的属性和方法，当我们调用实例方法时，我们用`instance.method()`来调用。能不能直接在实例本身上调用呢？在Python中，答案是肯定的。 任何类，只需要定义一个`__call()__`方法，就可以直接对实例进行调用。
    ```python
        class Student(object):
            def __init__(self, name):
                self.name = name

            def __call__(self):
                print('My name is %s.' % self.name)

        >>> s = Student('Michael')
        >>> s() # self参数不要传入
        My name is Michael.
    ```
- `__call__()`还可以定义参数。对实例进行直接调用就好比对一个函数进行调用一样，所以你完全可以把对象看成函数，把函数看成对象，因为这两者之间本来就没啥根本的区别。
- 判断一个变量是对象还是函数，可以用`callable()`函数进行判断。

### 使用枚举类
- 使用枚举类型定义一个class类型，然后，每个常量都是class的一个唯一实例。Python提供了`Enum`类来实现这个功能：
    ```python
    from enum import Enum
    Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))

    for name, member in Month.__members__.items():
        print(name, '=>', member, ',', member.value)
    ```
- `value`属性则是自动赋给成员的`int`常量，默认从`1`开始计数。
- 如果需要更精确地控制枚举类型，可以从`Enum`派生出自定义类：
    ```python
    from enum import Enum, unique=
    @unique
    class Weekday(Enum):
        Sun = 0 # Sun的value被设定为0
        Mon = 1
        Tue = 2
        Wed = 3
        Thu = 4
        Fri = 5
        Sat = 6
    ```
    `@unique`装饰器可以帮助我们检查保证没有重复值。

 ### 使用元类





[1]: https://baike.baidu.com/item/%E6%B1%89%E8%AF%BA%E5%A1%94/3468295
[2]: http://research.google.com/archive/mapreduce.html
[3]: http://baike.baidu.com/view/3784258.htm
[ 4 ]: https://docs.python.org/3/reference/datamodel.html
[ 5 ]: https://github.com/huangsam/ultimate-python

