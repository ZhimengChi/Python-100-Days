# Day 09 面向对象进阶
- getter & setter

python仅有公有和私有两种权限设置，getter&setter用于封装类时对属性进行绑定。
＊若使用getter（@property）则无法在没有对应setter（@＊＊.setter）的情况下进行解绑

- __slots__

用于限定类的绑定属性，仅对当前类起作用，对子类无作用。使用案例如下：

```Python
class Person(object):

    # 限定Person对象只能绑定_name, _age和_gender属性
    __slots__ = ('_name', '_age', '_gender')
```

- 静态方法和类方法

静态方法：@staticmethod，属于类而不属于实例（实例无法调用）

类方法：@classmethod，类方法的第一个参数约定名为cls，，它代表的是当前类相关的信息的对象（类本身也是一个对象，有的地方也称之为类的元数据对象）。

# Day 11 文件&异常

- 文件读写

| 操作模式 | 具体含义                         |
| -------- | -------------------------------- |
| `'r'`    | 读取 （默认）                    |
| `'w'`    | 写入（会先截断之前的内容）       |
| `'x'`    | 写入，如果文件已经存在会产生异常 |
| `'a'`    | 追加，将内容写入到已有文件的末尾 |
| `'b'`    | 二进制模式                       |
| `'t'`    | 文本模式（默认）                 |
| `'+'`    | 更新（既可以读又可以写）         |

如果不愿意在finally代码块中关闭文件对象释放资源，也可以使用上下文语法，通过with关键字指定文件对象的上下文环境并在离开上下文环境时自动释放文件资源。

```Python
def main():
    try:
        with open('致橡树.txt', 'r', encoding='utf-8') as f:
            print(f.read())
    except FileNotFoundError:
        print('无法打开指定的文件!')
    except LookupError:
        print('指定了未知的编码!')
    except UnicodeDecodeError:
        print('读取文件时解码错误!')


if __name__ == '__main__':
    main()
```

- JSON 读写：

json模块主要有四个比较重要的函数，分别是：

- `dump` - 将Python对象按照JSON格式序列化到文件中
- `dumps` - 将Python对象处理成JSON格式的字符串
- `load` - 将文件中的JSON数据反序列化成对象
- `loads` - 将字符串的内容反序列化成Python对象

# Day 12 字符串和正则表达式

| 符号               | 解释                                      | 示例             | 说明                                               |
| ------------------ | ----------------------------------------- | ---------------- | -------------------------------------------------- |
| .                  | 匹配任意字符                              | b.t              | 可以匹配bat / but / b#t / b1t等                    |
| \\w                | 匹配字母/数字/下划线                      | b\\wt            | 可以匹配bat / b1t / b_t等<br>但不能匹配b#t         |
| \\s                | 匹配空白字符（包括\r、\n、\t等）          | love\\syou       | 可以匹配love you                                   |
| \\d                | 匹配数字                                  | \\d\\d           | 可以匹配01 / 23 / 99等                             |
| \\b                | 匹配单词的边界                            | \\bThe\\b        |                                                    |
| ^                  | 匹配字符串的开始                          | ^The             | 可以匹配The开头的字符串                            |
| $                  | 匹配字符串的结束                          | .exe$            | 可以匹配.exe结尾的字符串                           |
| \\W                 | 匹配非字母/数字/下划线                    | b\\Wt            | 可以匹配b#t / b@t等<br>但不能匹配but / b1t / b_t等 |
| \\S                 | 匹配非空白字符                            | love\\Syou       | 可以匹配love#you等<br>但不能匹配love you           |
| \\D                 | 匹配非数字                                | \\d\\D           | 可以匹配9a / 3# / 0F等                             |
| \\B                 | 匹配非单词边界                            | \\Bio\\B         |                                                    |
| []                 | 匹配来自字符集的任意单一字符              | [aeiou]          | 可以匹配任一元音字母字符                           |
| [^]                | 匹配不在字符集中的任意单一字符            | [^aeiou]         | 可以匹配任一非元音字母字符                         |
| *                  | 匹配0次或多次                             | \\w*             |                                                    |
| +                  | 匹配1次或多次                             | \\w+             |                                                    |
| ?                  | 匹配0次或1次                              | \\w?             |                                                    |
| {N}                | 匹配N次                                   | \\w{3}            |                                                    |
| {M,}               | 匹配至少M次                               | \\w{3,}           |                                                    |
| {M,N}              | 匹配至少M次至多N次                        | \\w{3,6}          |                                                    |
| \|                 | 分支                                      | foo\|bar         | 可以匹配foo或者bar                                 |
| (?#)               | 注释                                      |                  |                                                    |
| (exp)              | 匹配exp并捕获到自动命名的组中             |                  |                                                    |
| (?&lt;name&gt;exp) | 匹配exp并捕获到名为name的组中             |                  |                                                    |
| (?:exp)            | 匹配exp但是不捕获匹配的文本               |                  |                                                    |
| (?=exp)            | 匹配exp前面的位置                         | \\b\\w+(?=ing)     | 可以匹配I'm dancing中的danc                        |
| (?<=exp)           | 匹配exp后面的位置                         | (?<=\\bdanc)\\w+\\b | 可以匹配I love dancing and reading中的第一个ing    |
| (?!exp)            | 匹配后面不是exp的位置                     |                  |                                                    |
| (?<!exp)           | 匹配前面不是exp的位置                     |                  |                                                    |
| *?                 | 重复任意次，但尽可能少重复 | a.\*b<br>a.\*?b | 将正则表达式应用于aabab，前者会匹配整个字符串aabab，后者会匹配aab和ab两个字符串 |
| +?                 | 重复1次或多次，但尽可能少重复 |                  |                                                    |
| ??                 | 重复0次或1次，但尽可能少重复 |                  |                                                    |
| {M,N}?             | 重复M到N次，但尽可能少重复 |                  |                                                    |
| {M,}?              | 重复M次以上，但尽可能少重复 |                  |                                                    |

Python提供了re模块来支持正则表达式相关操作，下面是re模块中的核心函数。

| 函数                                         | 说明                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| compile(pattern, flags=0)                    | 编译正则表达式返回正则表达式对象                             |
| match(pattern, string, flags=0)              | 用正则表达式匹配字符串 成功返回匹配对象 否则返回None         |
| search(pattern, string, flags=0)             | 搜索字符串中第一次出现正则表达式的模式 成功返回匹配对象 否则返回None |
| split(pattern, string, maxsplit=0, flags=0)  | 用正则表达式指定的模式分隔符拆分字符串 返回列表              |
| sub(pattern, repl, string, count=0, flags=0) | 用指定的字符串替换原字符串中与正则表达式匹配的模式 可以用count指定替换的次数 |
| fullmatch(pattern, string, flags=0)          | match函数的完全匹配（从字符串开头到结尾）版本                |
| findall(pattern, string, flags=0)            | 查找字符串所有与正则表达式匹配的模式 返回字符串的列表        |
| finditer(pattern, string, flags=0)           | 查找字符串所有与正则表达式匹配的模式 返回一个迭代器          |
| purge()                                      | 清除隐式编译的正则表达式的缓存                               |
| re.I / re.IGNORECASE                         | 忽略大小写匹配标记                                           |
| re.M / re.MULTILINE                          | 多行匹配标记                                                 |

例 替换不良内容：

```Python
import re


def main():
    sentence = '你丫是傻叉吗? 我操你大爷的. Fuck you.'
    purified = re.sub('[操肏艹]|fuck|shit|傻[比屄逼叉缺吊屌]|煞笔',
                      '*', sentence, flags=re.IGNORECASE)
    print(purified)  # 你丫是*吗? 我*你大爷的. * you.


if __name__ == '__main__':
    main()
```

爬虫其它选择： Soup（性能一般），Lxml

# Day 13 进程与线程

Unix和Linux操作系统上提供了fork()系统调用来创建进程，调用fork()函数的是父进程，创建出的是子进程，子进程是父进程的一个拷贝，但是子进程拥有自己的PID。fork()函数非常特殊它会返回两次，父进程中可以通过fork()函数的返回值得到子进程的PID，而子进程中的返回值永远都是0。Python的os模块提供了fork()函数。由于Windows系统没有fork()调用，因此要实现跨平台的多进程编程，可以使用multiprocessing模块的Process类来创建子进程，而且该模块还提供了更高级的封装，例如批量启动进程的进程池（Pool）、用于进程间通信的队列（Queue）和管道（Pipe）等。

例子见Day 13

多线程加锁：

```Python
from time import sleep
from threading import Thread, Lock


class Account(object):

    def __init__(self):
        self._balance = 0
        self._lock = Lock()

    def deposit(self, money):
        # 先获取锁才能执行后续的代码
        self._lock.acquire()
        try:
            new_balance = self._balance + money
            sleep(0.01)
            self._balance = new_balance
        finally:
            # 在finally中执行释放锁的操作保证正常异常锁都能释放
            self._lock.release()

    @property
    def balance(self):
        return self._balance


class AddMoneyThread(Thread):

    def __init__(self, account, money):
        super().__init__()
        self._account = account
        self._money = money

    def run(self):
        self._account.deposit(self._money)


def main():
    account = Account()
    threads = []
    for _ in range(100):
        t = AddMoneyThread(account, 1)
        threads.append(t)
        t.start()
    for t in threads:
        t.join()
    print('账户余额为: ￥%d元' % account.balance)


if __name__ == '__main__':
    main()
```

异步IO：多种IO模型

# Day 14 网络编程

TCP/IP四层模型，OSI/RM七层模型

TCP可靠传输原因：
- 数据不传错不传丢 （握手分手，校验，重传）
- 流量控制 （滑动窗口匹配sender和receiver之间的传输速度）
- 拥塞控制（RTT时间切片，滑动窗口）

#### 网络应用模式

1. C/S模式和B/S模式。这里的C指的是Client（客户端），通常是一个需要安装到某个宿主操作系统上的应用程序；而B指的是Browser（浏览器），它几乎是所有图形化操作系统都默认安装了的一个应用软件；通过C或B都可以实现对S（服务器）的访问。关于二者的比较和讨论在网络上有一大堆的文章，在此我们就不再浪费笔墨了。
2. 去中心化的网络应用模式。不管是B/S还是C/S都需要服务器的存在，服务器就是整个应用模式的中心，而去中心化的网络应用通常没有固定的服务器或者固定的客户端，所有应用的使用者既可以作为资源的提供者也可以作为资源的访问者。

requests库，待补充。。。

套接字，待补充。。。


