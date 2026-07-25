# TyepScript

## 变量类型注解

### 基础数据类型注解

```typescript
const age: number = 22
const name: string = '章鱼'
const boolean: boolean = true
const isNull: null = null
const isUndefined: undefined = undefined
```

### 数组数据类型注解

```typescript
// 这两种注解方法所达到的效果都是相同的
// 更推荐使用第一种方式进行注解，更加直观易读

// 第一种注解类型
const numbers: number[] = [1,2,3,4,5]
const numbers: (number | string)[] = [1,2,s,3,4]
// 第二种注解类型
const numbers: Array<number> = [1,2,3,4,5]
```

## 联合类型注解

当一个变量的类型不止一种的时候就可以使用联合类型，联合类型的语法为 | 

```typescript
const age: number | string = 22
const numbers: (number | string)[] = [1,2,3,g]
```

## 类型别名/接口

我们可以为一个多次使用的类型声明一个类型别名，以减少开发时多次声明类型的繁琐操作

类型别名和接口是两个不同的概念，但非常相似都可以为类型设置别名，在大多数情况下可以在它们之间自由选择

```typescript
type ResponseParams = {
	title: string,
	data: string
}

interface ResponseParams {
  title: string,
  data: string
}
```

两种方法声明的类型别名都可以进行拓展，但拓展方式不同

```typescript
type ResponseParams = {
	title: string
}

// type声明的类型别名使用 & 拓展类型
type ResponseData = ResponseParams & {
  data: string
}

interface ResponseParams {
  title: string
}

// interface声明的类型别名使用 extends 拓展类型
interface ResponseData extends ResponseParams {
  data: string
}
```

需要注意的是，type声明的类型别名，在声明完成之后无法修改，interface则没有此约束

```typescript
interface ResponseParams {
  title: string
}

// 向已声明的接口中增加新的字段注解
interface ResponseParams {
	data: string
}
```

## Function类型注解

```  typescript
// 参数类型注解
function TypeTest(value: string) {

}

// 返回值类型注解
function TypeReturnTest(): string {
    return 'Test'
}

// 箭头函数声明的方法使用类型注解
const TypeTest = (value: string): string => {
    return "Test"
}

// 还有为函数表达式声明类型注解的方式
// 实际开发中更推荐使用上面箭头函数的类型注解的方式
const TypeTest: (value: string) => string = (value) => {
  	return "Test"
}
// 或者
const TypeTest: (value: string) => string = function(value){
  	return "Test"
}
// 为函数表达式声明类型注解的方式更常用于声明函数的定义
const FunctionType = (value: string,params: string) => string

const format: FunctionType = (value,params) => {
  return "Test"
}
// 以及为形参内的函数提供类型注解
const functionParamsTest = (fn: (key: string,value: string) => string ) => {
  fn(key,value)
}
// 为类型别名内的方法提供类型注解
interface ParamsType {
  fn:(key: string,value: string) => string
}
```

## Type Guards / 类型守卫

```ts
// 基础类型的类型守卫
function TypeGuards(padding: number | string,input: string): string{
    return " ".repeat(padding) + input;
    // Argument of type 'string | number' is not assignable to parameter of type 'number'.
    // Type 'string' is not assignable to type 'number'.
}


// 使用自动类型收窄
function TypeGuards(padding: number | string,input: string): string{
    if(typeof padding === "number"){
        return " ".repeat(padding) + input;
    }
    return padding + input
}


// 实例的类型守卫
const TypeTestDom = document.queryselect(".typetestdom")

if(TypeTestDom instanceof HTMLElement){
    TypeTestDom.style.right = "0%"
}
```



## interface / 接口

``` typescript
// 接口
interface TypeTest {
    id: number,
    name: string,
    value: string,
}

// 接口数组
interface TypeTestList<TypeTest> {
    add: (obj: TypeTest) => void,
    get: () => Type
}
```



## Prototyep

### 在TypeScript中给原型对象新增方法时报错解决方法

#### 在需要使用的组件内导入即可

``` typescript
src/types/array.d.ts

interface Array<T> {
  sum(this: number[]): number
}



src/utils/array.ts

Array.prototype.sum = function (this: number[]) {
  return this.reduce((prev,curr) => {
    return prev + curr
  },0)
}
```
