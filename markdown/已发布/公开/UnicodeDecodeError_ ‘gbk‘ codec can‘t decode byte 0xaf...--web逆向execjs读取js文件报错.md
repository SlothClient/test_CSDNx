# 背景

做web逆向的时候我们通常是纯python模拟js思路或js+python直接逆向，第二种情况下我们要先获取到想要的js代码，js文件内测试接口后，通过python中的`execjs`模块实现相应接口的调用。通常我们会直接从网站扣下需要的代码（分析后硬扣或通过webpack），然后稍加删改和补环境就直接使用，但在学习逆向的过程中，为了帮助我们熟悉流程和思路，相当一部分人会有写注释的习惯。例如扣js并没有一次成功，每一错都在哪里犯错，哪里有坑，都会注释记录下来；亦或webpack有时候需要相关注释标记。

这不，在我上次敲代码的过程中，相同的代码，成功运行之后我在js文件中稍微写了些注释，改了下参数再次运行，诶，居然报错了。如下：

<img alt="" height="373" src="UnicodeDecodeError_ ‘gbk‘ codec can‘t decode byte 0xaf...--web逆向execjs读取js文件报错.assets/8dd9d5a29b6784b8.jpeg" width="1200" />

# 问题分析

刚开始看到这个报错，我的第一反应是我的参数不能这么改，毕竟请求到数据发生变化后会产生千奇百怪的报错，于是我将参数改回之前成功运行一样，再次运行，额，还是报错。

这么一排查，那就只剩一种情况了：文件中的注释导致的。我删去注释，再次执行，运行成功并获取到对应数据。

这么想来问题就解决了，我再创建一个readMe.md把我相关的注释说明一下就OK了。

# 深入分析

但是问题真的解决了吗，上面的解决方案很显然是在规避问题，掩耳盗铃，有一种惹不起我还躲不起吗的感觉。事后我再思考了一下，一般遇到unicod-edecode-error，看字面上的报错很显然是编码的问题。再分析整个逻辑中哪里使用到了编码，很显然就是编译js代码的时候：

# 其实这里有三步，可以分开写，分别是打开→读取→编译
compiledJS = execjs.compile(open("loadInfosJS.js").read())

一般人看到这估计就差不多了，初学python文件操作时都会遇到类似的错误，直接在open()方法中添加参数encoding='utf-8'即可，如果在此处不设置，默认采用平台依赖。感兴趣的小伙伴可以阅读源码，这里截取一部分：

'''
重点看这一句：
In text mode, if encoding is not specified the encoding used is platform dependent: locale.getpreferredencoding(False) is called to get the current locale encoding.
'''

def open(file, mode='r', buffering=None, encoding=None, errors=None, newline=None, closefd=True): # known special case of open
    """
    Open file and return a stream.  Raise OSError upon failure.
    
    file is either a text or byte string giving the name (and the path
    if the file isn't in the current working directory) of the file to
    be opened or an integer file descriptor of the file to be
    wrapped. (If a file descriptor is given, it is closed when the
    returned I/O object is closed, unless closefd is set to False.)
    
    mode is an optional string that specifies the mode in which the file
    is opened. It defaults to 'r' which means open for reading in text
    mode.  Other common values are 'w' for writing (truncating the file if
    it already exists), 'x' for creating and writing to a new file, and
    'a' for appending (which on some Unix systems, means that all writes
    append to the end of the file regardless of the current seek position).
    In text mode, if encoding is not specified the encoding used is platform
    dependent: locale.getpreferredencoding(False) is called to get the
    current locale encoding. (For reading and writing raw bytes use binary
    mode and leave encoding unspecified.)
    ...
    ...
    """

也就是将我的代码改为下面这样，js文件中就能正常写注释了。

compiledJS = execjs.compile(open("loadInfosJS.js",encoding='utf-8').read())

#  关键点总结

报错分析

文件操作

善读源码

---

## Cover 图

![cover_1](UnicodeDecodeError_ ‘gbk‘ codec can‘t decode byte 0xaf...--web逆向execjs读取js文件报错.assets/677bbb2f33d34cc2.png)
