# Methods

## 调用方式

### Instance Methods

必须通过[实例](https://developer.mozilla.org/zh-CN/docs/Glossary/Instance)调用

### Static Methods

直接由[构造函数](https://developer.mozilla.org/zh-CN/docs/Glossary/Static_method)直接调用

## String常用方法

### Instance Methods

``` javascript
let str = "JavaScript"

/**
 * 返回字符串长度
 * @returns number
 */
str.length

/**
 * 转换为小写
 * @returns string
 */
str.toLocaleLowerCase()

/**
 * 转换为大写
 * @returns string
 */
str.toLocaleUpperCase()

/**
 * 返回传入值对应下标的数据
 * @returns string
 */
str.charAt()

/**
 * 返回传入值对应下标的UniCode码
 * @returns number
 */
str.charCodeAt()

/**
 * 截取对应区间的子字符串
 * 包括Start 不包括End
 * strat,end 值为负数时 取值为 (length + start),(length + end)
 * @returns string
 */
str.slice(start,end)

/**
 * 截取对应区间的子字符串
 * 包括Start 不包括End
 * 第一个参数为负数时直接转换为0
 * 第二个参数为可选值 为空时自动默认输出从Start到结尾的子字符串
 * 第二个值为负数时直接转换为0
 * @returns string
 */
str.substring(start,end)

/**
 * 截取对应区间的子字符串
 * @param start - 起始位置
 * @param length - 截取长度
 * @returns string
 */
str.substr(2,10)

/**
 * 字符串结尾添加新字符串
 * 平常不用 直接 str + "新字符串"
 * @returns string
 */
str.concat("")

/**
 * 去除首尾空格
 * @returns string
 */
str.trim()

/**
 * 去除首空格
 * @returns string
 */
str.trimStart()

/**
 * 去除尾空格
 * @returns string
 */
str.trimEnd()

/**
 * 替换对应字符串
 * 可使用正则表达式
 * @param 匹配项 - 要匹配的字符串或正则
 * @param 替换值 - 替换的字符串
 * @returns string
 */
str.replace(匹配项,替换值)
str.replace(/a/,"****")//替换全部的a

/**
 * 分割字符串 转换为数组
 * @param 用于分割字符串的字符串
 * @returns string[]
 */
str.split("用于分割字符串的字符串")

```

## Array常用实例方法

### Instance Methods

``` javascript
/**
 * 将values添加到数组的开头
 * @returns arr.length
 */
arr.unshift(values)

/**
 * 将values添加到数组最后一项
 * @returns arr.length
 */
arr.push(values)

/**
 * 删除数组的第一个元素
 * @returns 被删除的元素，数组如果为空返回undefined
 */
arr.shift()

/**
 * 删除数组的最后一个元素
 * @returns 被删除的元素，数组如果为空返回undefined
 */
arr.pop()

/**
 * 删除数组的最后一个元素
 * @param {number} start - 起始下标
 * @param {number} [deleteCount] - 删除数量（可选）为0则不删除
 * @param {...any} [items] - 要插入的元素（可选）
 * @returns Array 一个包含了删除的元素的数组。
 * 如果只删除了一个元素，则返回一个元素的数组
 * 如果没有删除任何元素，则返回一个空数组
 */
arr.splice(start, deleteCount,items)

/**
 * 同splice 但此方法不会改变原数组，而是创建一个修改后的数组作为返回值
 * 如果原数组是稀疏的，在新数组中空槽会被替换为undefined
 */
arr.toSplice(start, deleteCount, items)


/**
 * 遍历循环
 * @returns undefined
 */
arr.forEach((item, index, array) => {

})

/**
 * 筛选
 * 如果返回值为真，将此项数据浅拷贝
 * @returns array
 */
arr.filter((item,index,array) => {

})

/**
 * 返回满足测试函数的第一个元素的值
 * @returns value | undefined
 */
arr.find((value,index,array) => {

})

/**
 * 返回满足测试函数的第一个元素的索引
 * @returns index | -1
 */
arr.findIndex((value,index,array) => {

})

/**
 * 判断数组是否包含指定值
 * @returns boolean
 */
arr.includes(searchElement,fromIndex?)


// 返回迭代对象（同Map）
             
/**
 * 返回key(index)的迭代对象
 * @returns Iterator
 */
arr.keys()
             
/**
 * 返回valude的迭代对象
 * @returns Iterator
 */
arr.values()

/**
 * 将数组内的所有数据拼接成字符串返回
 * @returns String
 */
arr.join('/')

/**
 * @callback 
 * @param element - 当前元素
 * @param index - 当前索引
 * @param array - 当前数组
 * @returns 返回处理后的新数组
 */
arr.map((element,index,array) => {
    return 
})
```

## Map常用实例方法

### Instance Methods

``` javascript
/**
 * 添加/修改
 * 返回值为整个map对象
 * @returns map
 */
map.set(key,value)

/**
 * 获取
 * 返回值为该键对应的值或undefined
 * @returns value | undefined
 */
map.get(key)

/**
 * 删除指定元素
 * 元素存在且移除返回true 元素不存在返回false
 * @returns boolean
 */
map.delete(key)
/**
 * 清除map中所有元素
 * @returns undefined
 */
map.clear()

/**
 * getOrInsert
 * 如果map中存在该键则返回对应的value，如果不存在则添加该键设置并返回defaultValue
 * @returns value | defaultValue
 */
map.getOrInsert(key,defaultValue)

/**
 * 循环遍历
 * @returns undefined
 */
map.forEach((value,key,map) => {

})

// 返回迭代对象

/**
 * 返回key的迭代对象
 * @returns Iterator
 */
map.keys()

/**
 * 返回valude的迭代对象
 * @returns Iterator
 */
map.values()

/**
 * 返回key，value的迭代对象
 * @returns Iterator
 */
map.entries()
/**
 * 查询指定元素
 * 方法返回一个布尔值，如果对象中具有指定键的元素，则返回true
 * @returns boolean
 */
map.has(key)

```

# Promise

## Async Await

### 使用Async声明异步函数

``` typescript
const Promise = async (() => {
    //使用async声明的异步函数返回对象为Promise对象
})
```

### Async需搭配Await使用

``` typescript
const Promis = async ((value:String) => {
    //可使用await调用异步函数
    //await会等待Promise完成之后返回最终的结果
    await axios.get('',{
        params:{
            value : value
        }
    })
    //被await声明的函数或方法，在被调用时，需等待Promise执行完毕后才会被调用
})
```
