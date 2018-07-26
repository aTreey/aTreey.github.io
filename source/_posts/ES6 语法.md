# JS ES6 语法

### 1. let var 变量，作用域

    var a = 10;

    if (a > 10) {
        let b = 5; // let 只能在代码块内被访问

        console.log(b);
    }

    // 访问 b 报错
    console.log(b);
    console.log(a);


### 2. const 只能分配一次值

{
    const const_a = 1;
    console.log(const_a)

    // 报错
    const_a = 2;

    // 声明一个常量数组
    const const_array = [];
    const_array.push(1);
    const_array.push(2);
    // 再次赋值时会报错
    const_array = [3];

}

### 3. 解构语法
{
    // 1.解构数组
    function getArray() {
        return [10, 20, 30];
    }

    let [a,b,c] = getArray()
    console.log(a, b, c)


    // 函数返回一个对象
    function getObj() {
        return {dessert: '面包', drink: '水', play: 'Game'};
    }

    // 2.解构对象
    let {des: dessert,  dri: drink, pla: play} = getObj()
    console.log(des, dri, pla);
}

### 4. 模版字符串
{
    let name = '姓名：', password = '密码：'
    let str_4 = name + '李老师' + '----' + password + '北京太热。。。'
    console.log(str_4)

    // 使用 `${}` 包裹会生成字符串
    let str_41 = `${name}李老师----${password}北京太热`
    console.log(str_41)

    // 多行显示时直接回车换行
    let str_42 = `${name}李老师
    ----
    ${password}北京太热`
    console.log(str_42)
}

###  5. 带标签的模版字符串

{
    let name = '姓名：', password = '密码：'
    // 多行显示时直接回车换行
    let str_5 = strTest`${name}李老师 ---- ${password}北京太热`
    console.log(str_5)


    // strings 字符中包含的被插值分割的子字符串已数组形式展现
    // values 字符串中的插值数组
    function strTest(strings, ...values) {
        console.log(strings)
        console.log(values)
    }

    function strTest1(strings, ...values) {
        let result = '';
        for (let i = 0; i < values.length; i++) {
            result += strings[i]
            result += values[i]
        }
        return result
    }
}

###  6. 判断字符串是否包含其他字符

{
    let name = '姓名：', password = '密码：'
    let str_6 = `${name}李老师---${password}北京真热`
    console.log(str_6.startsWith(`${name}`))
    console.log(str_6.endsWith(`${name}`))
    console.log(str_6.includes('李老师'))
}

###  7. 函数默认参数
{
    function funcDefaultParame(param1 = 'a', param2 = 'b', param3 = 'c') {
        return `${param1}--${param2}--${param3}`
    }
    console.log(funcDefaultParame(1, 2,3))
}


###  8. "..."展开操作符


{
    let a = [1,2,3];
    console.log(a);
    console.log(...a)

    let b = [...a, 4, 5];
    console.log(a);
    console.log(b)
}

###  9. '... '剩余操作符, 一般用在函数参数， 可以看作是一个可以展开的参数


    //  ... param3表示函数可以接收除了param1, param2,两个参数外，多个参数，都会放在param3中
    function operator(param1, param2, ...param3) {
        console.log(param1, param2, param3)
    }
    operator('a','b','c','d','e','f','g')




###  10. 解构对象解构函数

    // 当函数参数是对象是可以按照对象解构
    function getObjcValue(param1, param2, {value1, value2} = {}) {
        console.log(param1, param2, value1, value2)
    }

    getObjcValue(1,2,{value1: -3,value2: -4})



###  11. 获取函数名称


    function getObjcValue(param1, param2, {value1, value2} = {}) {
        console.log(param1, param2, value1, value2)
    }
    console.log(getObjcValue.name)

    let funName = function getfunctionName() {  }
    console.log(funName.name)


###  12. 箭头函数


    // 原来普通写法
    var funType = function arrowfunction(param) {
        return param
    }

    // 函数参数 => 函数返回值
    // '=>' 的左边表示箭头函数的参数，多个时表示为 (param1, param2, param3)
    // '=>' 的右边表示箭头函数的返回值，如果返回值是一个对象时用 { } 表示
    let arrowFunction = (param1, param2) => {
        return `${param1}${param2}`
   
   
}