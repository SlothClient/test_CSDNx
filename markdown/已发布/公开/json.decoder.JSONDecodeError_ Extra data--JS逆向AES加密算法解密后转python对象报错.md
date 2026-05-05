# 背景

今天做web逆向的时候一直没找到想要的接口，调出开发者工具对网络请求筛查了一遍，找到一个类似的接口，进去看了一下，很朴素的请求，返回的数据是司空见惯的密文，想着闲着没事解一下。

# 问题产生

首先定位到解密入口，短暂的分析得知开发者使用的是AES算法的ECB模式，按照开发者的逻辑打断点调试很容易搞到了ECB的密钥key。接下来就是python复现请求逻辑，这是我借鉴网上代码写的解密函数。

def decrypt_oralce(text,key):
    # 密钥
    key = key.encode()
    # 初始化加密器
    aes = AES.new(key, AES.MODE_ECB)
    #优先逆向解密base64成bytes
    base64_decrypted = base64.b64decode(text)
    #执行解密密并转码返回str
    decrypted_text = aes.decrypt(base64_decrypted)
    return decrypted_text

接着直接发请求获取密文然后调用函数解密，接着使用json.loads()将json串转为python中的字典对象打印输出，运行，小功告成！

我本来是这么想的，结果居然报错了，这么简单的逻辑居然都报错！

这是错误提示：

<img alt="" height="281" src="json.decoder.JSONDecodeError_ Extra data--JS逆向AES加密算法解密后转python对象报错.assets/6ae72280789fe563.png" width="1200" />

# 解决问题

出错怎么办？上网查呗。

接着我b度了一下，翻了好几个回答，都是多、乱、杂，看得眼花缭乱，不想继续看下去。

于是我回到程序中，将解密的结果with open()保存到一个文件中，检查一下，定睛一看，数据末尾有好几个\x0c！！

这不就破案了吗，很明显的没有解填充，于是我给解密函数加了一行，如下

def decrypt_oralce(text,key):
    # 密钥
    key = key.encode()
    # 初始化加密器
    aes = AES.new(key, AES.MODE_ECB)
    #优先逆向解密base64成bytes
    base64_decrypted = base64.b64decode(text)
    #执行解密密并转码返回str
    decrypted_text = aes.decrypt(base64_decrypted)
    #解填充
    decrypted_text = unpad(decrypted_text, AES.block_size).decode()
    return decrypted_text

运行，成功！

---

## Cover 图

![cover_1](json.decoder.JSONDecodeError_ Extra data--JS逆向AES加密算法解密后转python对象报错.assets/6ae72280789fe563.png)
