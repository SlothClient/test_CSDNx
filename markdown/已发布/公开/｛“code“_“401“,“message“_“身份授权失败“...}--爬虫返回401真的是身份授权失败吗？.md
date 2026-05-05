# 背景

前几天做web逆向的时候，按照流程我先对接口进行数据返回测试，结果跌碎我的眼镜，压根没请求到数据，直接返回了json串｛"code":"401","message":"身份授权失败"...}

<img alt="" height="110" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/47/47f681898a5514ba7b7d48c7a1be3d507f930558dcea77120f3014a0160413c9.png" width="1200" />

# 前期问题分析

身份授权失败，看到这个词我就开始想是否是User-Agent、Cookie和Referer的问题，请求头三剑客嘛毕竟，看了下curl生成的请求代码，发现没有cookie，然后回到网站使用开发者工具点开application发现存在动态生成的两套cookie，一个首页产生的，另外一个某个请求链接产生的，然后我理所当然使用session请求

一顿操作猛如虎，一看返回401，还是没解决，靠！

没办法，度娘搜一下，看了下几个平台回复，大致上说是请求头中的authority和host在作祟，回网站看了下，authority没有，host还真有，前面一段貌似还是编码过后的；再看看代码里的前请求头，嘿，还真没有，我立马给他copy上，再次请求，再次401。。。

又陪度娘喝了会茶，没有有效解决方案。后面找到了绕指纹的curl_cffi试了下，居然能成功绕过所谓的身份验证。这个不是文章重点，不再详述。

# 重点问题分析：requests.post(data/json)

后期发现可以不做指纹绕过，但是是一个非常细节的点。

网站的载荷，也就是请求体，一般我们查看的都是编译之后的对象，这样固然方便阅读，但不利于我们查看真实请求的情况，开发者工具为我们提供了view source查看原数据

编译后 → 格式规范，存在空格

<img alt="" height="92" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/95/959d2e211f4939a9d0ddb16f7a0fdd04aa0a42ddfe58375aafe922079568444f.png" width="1200" />

编译前 → 类似压缩格式，不存在空格

<img alt="" height="104" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/76/769d5981853b26745336ba5bbcf55b3e34ccdda5b31ce2eeac581d8d69b79868.png" width="989" />

## post请求的json形参

原理上，json=json_data会将json_data(python中的字典)做一步json.dumps(json_data)转换成json串，这是前置知识。

划重点！！！json.dumps()存在默认格式，也就是键值对之间使用", "分隔，键与值之间使用": "分隔，当然，形参列表中存在形参可以对格式进行调整，我将我的json_data调整为了网站原格式。

## post请求的data参数

接下来我直接使用data=[转换后的json_data]，因为使用json=[data]会默认帮你dumps一下，data则是直接传递请求体。

注释掉指纹绕过，再次请求，仍然是成功。

<img alt="" height="104" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/6b/6b08b3022f8cec63799badd23864f836163831a16033c29fa052030a1e8bd43d.png" width="1200" />

最后传统逆向步骤完成解sign即可。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/13/13e57b7e4f49ec7bed929e62890d74c96c2add2e982d04f7df60314a2ebbe0d0.png)
