js 学习

## 数据类型

```
var fullName;
//  undefined

fullName
// undefined

fullName + 2
// NaN

```

- 判断类型名称

	```
	fullName = '张'
	"张"
	typeof(fullName)
	// "string"
	```
	
	```
	var weight = 100;
	undefined
	typeof(weight)
	// "number"
	
	```
	
	
	```
	var firstName = '王', lastName = '张'
	undefined
	firstName + lastName
	// "王张"
	```

- 类型不相同时转换后在拼接 
	
	```
	var weightIncrease = '2.5斤'
	undefined
	weight + weightIncrease
	// "1002.5斤"
	```

## 数组


	
	var array = [];
	undefined
	array.length
	// 0
	typeof(array)
	// "object"
	
	

- 增加元素

	```
	array.push('1')
	// 4
	array
	// (4) ["ab", "cbd", "fcg", "1"]
	
	```

- 数组前面添加

```
array.unshift('0')
// 6
array
// (6) ["0", "ab", "cbd", "fcg", "1", Array(2)]
```

- 删除最后元素

	```
	array.pop() // 返回删除的元素
	// (2) ["2", "3"]
	array
	// (5) ["0", "ab", "cbd", "fcg", "1"]
	```
- 删除第一个元素

	```
	array.shift()
	// "0"
	array
 	// (4) ["ab", "cbd", "fcg", "1"]
	```	
- 删除指定的值，但数组元素没有被删

	```
	delete array[2]
	// true
	array
	// (4) ["ab", "cbd", empty, "1"]
	```	
	
- 删除指定的元素

	```
	array.splice(1)
	// (3) ["cbd", empty, "1"]
	array
	// ["ab"]
	```
	
## 函数

### 调用调用

```
alertMessage();

function alertMessageWithParamter(message) {
    alert(message)
}

alertMessageWithParamter('有参数的函数')
alertMessageWithParamter(250)
alertMessageWithParamter(
    console.log('test')
)

```
### 函数表达式，使用函数声明的方式

```
var alertMessage_expression = function expression_alert (message) {
    alert(message)
}

alertMessage_expression('匿名函数调用')

expression_alert('函数表达式')	
```