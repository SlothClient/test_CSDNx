遇到该错误一般是你在使用`python`中的`makedirs`所创建的文件夹名称不合法。
`windows`中创建非法名称文件夹会有以下提示：
![在这里插入图片描述](File “D_Python310_lib_os.py“,line 225,in makedirs mkdir(name,mode) OSError_[winError 123]文件名、目录名.assets/e2f6b7f709e5abc8.png)
所以建议在使用`os.makedirs()`前先判断自己输入/自动获取的文本是否支持作为文件名。可以创建简单函数进行判断：

```python
def is_supported(dir_name):
    unsupported_char_list = ["\\", "/", ":", "*", "?", "\"", "<", ">", "|"]
    for char in unsupported_char_list:
        if char in dir_name:
            return 0
    return 1
```
在逻辑代码中加入这两行：

```python
if not is_supported(title):
    title = input(f'“{title}”不可作为文件名（请注意文件名不能包含下列任何字符/:*?"<>），请输入合法名：')
```
即可解决。
