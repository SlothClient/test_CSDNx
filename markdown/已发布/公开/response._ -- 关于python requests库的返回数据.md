在Python的requests库中，response对象代表了一个HTTP响应，它提供了多种方法来访问响应的内容。

### 1. response.text

response.text是一个字符串，它包含了HTTP响应的主体（body）的文本内容。这个文本内容是根据HTTP响应头中的Content-Type字段来解码的。如果Content-Type是application/json，requests会尝试使用UTF-8编码来解码响应体，但这并不意味着它会将JSON字符串转换为Python对象。它只是简单地将字节数据转换为字符串。

### 2. response.json()

response.json()是一个方法，它尝试将响应体中的JSON字符串解析为Python对象（通常是字典或列表）。如果响应体不是有效的JSON，这个方法将抛出一个ValueError或json.JSONDecodeError异常。这个方法非常有用，因为它允许你以Python字典或列表的形式直接访问JSON数据，而无需手动解析JSON字符串。

### 3. response.text() 和 response.json（作为方法被误解为属性）

- 
	response.text() 实际上是不存在的。response.text是一个属性，不是一个方法，所以你不能像调用函数那样调用它（即response.text()是错误的）。

	

	- 
	response.json() 是正确的方法调用方式，用于将JSON响应体解析为Python对象。

	

### 总结

- 
	使用response.text来获取响应体的原始文本内容（作为字符串）。

	

	- 
	使用response.json()来将响应体中的JSON字符串解析为Python对象（如字典或列表）。

---

## Cover 图

![cover_1](response._ -- 关于python requests库的返回数据.assets/5423a0714691e90c.png)
