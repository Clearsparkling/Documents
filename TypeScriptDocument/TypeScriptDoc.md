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

### 对象类型注解

```typescript
const Object: {
  key: string,
  value: string,
  fn(key: string): void,
  // 函数的注解还有第二种方式
  fn:(key: string) => voidx
}
```

### 元组类型注解

元组类型是另一种类型的数组，它确切地知道包含多少个元素，以及特定索引对应的类型

> [!NOTE]
>
> 元组声明的数组，必须严格按照元组类型注解的规范。特定的索引必须是特定的类型。

```typescript
let position: [number, number] = [39.5427, 116.2317]
```

### 类型断言

在获取某些Element对象时，TS的类型推断都是推断为**Element | null**但常常我们获取的ELement对象都是继承自Element的其他对象类型，有许多属性是Element对象上没有的，此时我们想要获取或更改这个属性时，TS的类型检查就会报错提示该类型上不存在该属性，常常我们都知道获取的元素是何种对象类型，我们就可以使用断言来指定类型。

```typescript
const aElement = document.querySelector('.element')
aElement.value <---- 此时TS的类型检查会报错
x
const aElement = document.querySelector('.element') as HTMLAnchorElement
aElement.value <---- 此时TS的类型检查就不会报错
```

> [!WARNING]
>
> 但在实际开发中，并不推荐使用断言这种写法来解决这种问题，应使用TS的类型收窄来解决这类问题，见下方[类型守卫](#type-guards---类型守卫)

### 字面量类型注解

使用const声明的字符串类型，TS的类型推断会推断为常量，所以声明的常量的类型就是字面量类型

```typescript
let str = 'Hello TS'  ---> str: string
const str = 'Hello TS'  ---> str: 'Hello TS'
```

单独使用时，并不是那么的常用，我们常常将字面量注解和联合类型注解一起使用，用来声明该变量可出现的所有值

```typescript
const str: 'left' | 'right' | 'up' | 'down'
```

### 枚举类型

枚举类型定义一组命名常量

在没有定义value时候，枚举类型里的成员将会从0开始自动递增,在前面的成员初始化number类型的值后，后续初始化的成员，将会根据最后一个已定义的成员的值自动递增每次+1

```typescript
enum TypeTest {
  Up,   ---> 0
  Down,   ---> 1
  Left,   ---> 2
  Right   ---> 3
}

enum TypeTest {
  Up = 1,
  Down,   ---> 2
  Left,   ...
  Right
}
```

每个枚举成员都有一个值，可以是常数值或计算值，在枚举成员没有初始化时，成员的值如上述所示

字符串枚举

```typescript
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT'
}
```

使用Object也可以代替Enum，使用Object的代码更接近原生JavaScript代码语法，且打包时编译复杂度更低

```typescript
export const Role = {
  Up = 'UP',
  Down = 'DOWN',
} as const

export type Role = typeof Role[keyof typeof Role]
```

### Any类型

> [!WARNING]
>
> 不推荐使用Any！！！这会让TypeScript变为"AnyScript"，失去了使用TypeScript的任何优势
>
> 使用Any注解时，没有任何的代码提示，这意味着不会有任何的类型错误提示，即使可能存在错误！

```typescript
let obj: any = {x: 0}

// 及时访问不存在的属性
obj.aaa 
obj()
obj = '1'
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

## TypeScript高级类型

### class类

在TS中类的创建方式同JS完全相同，完全兼容ES6的语法

```typescript
class Person {
 	// 实例属性初始化
  age: number, // number
  gender = '男' // string
}

const person = new Person() // 此时person的类型就是Person
```

### Class Heritage

#### Extends(拓展)

类的继承方式同JS中一致使用extends(继承)关键字实现

```typescript
class Animal{
  move() { console.log('Moving along!') }
}

class Cat extends Animal{
  bark() { console.log('大狗，大狗，叫！！！！') }
}

const cat = new Cat()
```

#### implements(实现)

接口的实现同java中的接口一样，需要实现接口定义中的所有方法，否则将报错

```typescript
interface Singable {
  sing(): void
}

class Person implements Singable {
  sing() {
    console.log('哒哒哒 哒哒')
  }
}
```

### Member Visibility

成员可见度的声明和Java中近乎完全一样

#### public

公共的公开的成员，可以在任意位置被访问

```typescript
class publicClass {
  public member: string = '公开的'
}

const publicclass = new publicClass()
publicclass.member
```

#### protected

受保护的成员，仅可在当前类和其子类（非实例对象！）中可见

```typescript
class Person {
  protected move() { console.log('Moving along') }
}

class ZhangSan extends Person {
  bark() {
    console.log('fucker!!!')
    move()
  }
}

const person = new Person()
person.move() // 此处会报错
```

#### private

私有的成员,仅在当前类中可见

```typescript
class Animal {
  private __run__() {
    console.log('内部函数')
	}
  
  move() {
    __run__() // 此处可用
  }
}

const animal = new Animal()
animal.__run__() // 此处不可用

class Cat extends Animal {
  move() {
    __run__() // 此处不可用
  }
}
```

### readonly

只读修饰符，只读用来防止构造函数之外对属性进行赋值。

```typescript
class Person {
  readonly age: number = 18
  constructor(age: number) {
    this.age = age
  }
}
```

```typescript
interface IPerson {
  readonly name: string
}

let obj: IPerson = {
	name: 'clearsparkling'
}

obj.name = 'rose' // error
```

```typescript
let obj:{ readonly name: string } = {
	name: 'clearsparkling'
}

obj.name = 'rose' // error
```

> [!WARNING]
>
> 只读修饰符只能用来修饰属性，不能用来修饰方法
>
> 在为有默认值的只读属性声明时，一定要为属性声明类型注解，否则TS将自动识别成字面量类型，在构造函数内对其赋值的时候就会报错

### 类型兼容性

#### Class类型兼容性

1.Structural Type System(结构化类型系统)

2.Nominal Type System(标明类型系统)

TS采用的是结构化类型系统，也叫做duck typing(鸭子类型)，类型检查关注的是值所具有的形状，也就是说，在结构类型系统中，如果两个对象具有相同的形状，则认为它们属于同一类型。

```typescript
class Point {
    x: number
    y: number
    constructor(x:number,y:number) {
        this.x = x
        this.y = y
    }
}

class Point2D {
    x: number
    y: number
    constructor(x: number, y: number) {
        this.x = x
        this.y = y
    }
}

const point:Point = new Point2D(1,2)
```

对于对象类型来说，y的成员至少与x相同，则x兼容y(成员多的可以赋值给少的)

```typescript
class Point {
    x: number
    y: number
    constructor(x: number, y: number) {
        this.x = x
        this.y = y
    }
}

class Point3D {
    x: number
    y: number
    z: number
    constructor(x: number, y: number, z: number) {
        this.x = x
        this.y = y
        this.z = z
    }
}

const point: Point = new Point3D(1, 2, 3)

const point3D: Point3D = new Point(1, 2)
```

#### interface类型兼容性

对于接口的类型兼容性，同class类似

#### function类型兼容性

function的类型兼容性在俩个function的部分形参相等的时，**形参少的function可以赋给形参多的**

```typescript
type F1 = (a: number) => void
type F2 = (a: number, b: number) => void

let f1: F1
let f2: F2

f1 = f2 // error
f2 = f1
```

函数返回值的类型兼容性参照[对象类型兼容性](#class类型兼容性)的规则

```typescript
type F1 = () => { name: string }
type F2 = () => { name: string, age: number }

let f1: F1
let f2: F2 = () => {
    return { name: 'clear', age: 11 }
}

f1 = f2
f2 = f1 // error
```

#### 交叉类型

用于组合多个类型为一个类型，同接口继承很相似

```typescript
interface Person {
    name: string
}

interface Contact {
    phone: string
}

type PersonDetail = Person & Contact

let obj: PersonDetail = {
    name: 'clear',
    phone: '18370812892'
}
```

接口集成与交叉类型对于同名成员之间的类型冲突处理方式不同。

##### 接口继承

```typescript
interface A {
    fn: (value: string) => void
}

interface B extends A { // error 
  // 接口“B”错误扩展接口“A”。
  // 属性“fn”的类型不兼容。
  //  不能将类型“(value: number) => void”分配给类型“(value: string) => void”。
  //    参数“value”和“value” 的类型不兼容。
  //      不能将类型“string”分配给类型“number”。
    fn: (value: number) => void
}
```

##### 交叉类型

```typescript
interface A {
    fn: (value: string) => void
}

interface B {
    fn: (value: number) => void
}

type C = A & B
// 可以简单理解为
// fn: (value: string | number) => void
// 实际上是方法的重载
// fn: ((value: string) => void) & ((value: number) => void)
```

### 泛型

常常我们写的一个function可以传入多种类型的参数，但如此标注function的类型就有些困难了，我们可以使用泛型在保证类型安全的前提下，让函数与多种类型一起工作，从而实现复用。

```typescript
// 创建一个id function，传入什么数据就返回该数据本身（也就是说，参数和返回值类型相同）
function id(value: string): string {
  return value
}
// 但如果我们在调用时传入一个number类型的参数，TS就会报错提示类型错误。
// 但我们依旧想实现这种效果，先前只能将类型标注为any，这样就失去了TS的类型检查，这是不安全也不建议的
function id(value: any): any {
  return value
}

// 这时候泛型就能解决上述想要解决的问题
// 我们用尖括号声明一个泛型
function fn<Type>(value: Type): Type {
  return value
}

// 在调用时同样使用尖括号来描述泛型
fn<number>(1)
fn<string>('ClearSparkling')
// 这样在方法中Type的类型就是在调用时传入的类型
```

#### 简化调用泛型函数

在调用泛型时TS内部会采用一种**类型参数推断**的机制，来根据传入的实参自动推断出类型变量的Type的类型

```typescript
function id<T>(value: T): T {
    return value
}
id<number>(1)
// function id<number>(value: number): number
id(1)
// function id<1>(value: 1): 1
```

> [!NOTE]
>
> 就上面的示例来说TS的类型推断 T为字面量类型
>
> 如果觉得TS的类型参数推断的不准确，我们可以自行声明传入的参数类型。

#### 泛型约束

##### Extends

默认情况下，泛型函数的类型变量Type可以代表多个类型，这导致无法访问任何属性。

比如，id('a')调用函数时获取参数的长度：

```typescript
function id<Type>(value: Type): Type {
  console.log(value.length)
  return value
}
```

Type可以代表任意类型，无法保证一定存在length属性，比如number类型姐没有length

此时就需要为泛型添加约束来收缩类型。

通过类型约束来要求传入的参数必须具有length属性

也是通过extends来约束

```typescript
interface ILength {
    length: number
}

function id<T extends ILength>(value: T): T {
    console.log(value.length)
    return value
}

id({ length: 10 })
id('clearsparkling')
id(['a','b'])
```

##### Keyof

泛型的类型变量可以有多个，并且**类型变量之间还可以约束**（比如，第二个类型变量受第一个类型变量约束）。

```typescript
function getProp<Type, Key extends keyof Type>(value: Type, key: Key) {
    return value[key]
}

let person = { name: 'clearsparkling', age: 22 }

getProp(person,)
// function getProp<{
//  name: string;
//  age: number;
// }, "name" | "age">(value: {
//  name: string;
//  age: number;
// }, key: "name" | "age"): string | number

getProp(person, 'name')
```

#### 泛型接口

接口同样也可以配合泛型来使用，以增加其灵活性，增强复用性。

```typescript
interface IdFunc<Type> {
    id: (value: Type) => Type
    ids: () => Type[]
}

let obj: IdFunc<number> = {
    id(value) { return value },
    ids() {
        return [1,2,3]
    },
}
```

> [!NOTE]
>
> 使用泛型接口时，必须显式的指定具体的类型！！！

#### 泛型类

创建泛型类

```typescript
class GenericNumber<T> {
	defaultValue: T
  add: (x: T, y: T) => T
}
```

### Partial

Partial可以用来构造(创建)一个类型，将Type的所有属性设置为可选。

```typescript
interface Props {
    id: string
    children: number[]
}

type PartialProps = Partial<Props>
```

Partial创建的可选属性只是浅层的可选

Partial配合上泛型使用

```typescript
interface ArrayType<T> {
    data: T[]
    forEach: (callbackFn: (value: T, index: number, array: T[]) => void, thisArg?: any) => void
}

type PartialArrayType<T> = Partial<ArrayType<T>>
```

> [!NOTE]
>
> [Partial的实现](#partial的实现)

### Readonly

Readonly可以用来构造(创建)一个类型，将Type的所有属性设置为只读

```typescript
interface Props {
    id: string
    children: number[]
}

type PropsReadonly = Readonly<Props>
```

### Pick

Pick<Type,Keys>从Type中选择一组属性来构造新类型

```typescript
interface Props {
    id: string
    title: string
    children: number[]
}

type PropsPick = Pick<Props, 'id' | 'title'>

// PropsPick的类型
type PropsPick = {
 title: string;
 id: string;
}
```

### Record

Record<Keys,Type>构造一个对象类型，属性键为Keys，属性类型为Type

```typescript
type RecordType = Record<'a' | 'b' | 'c', string[]>

// RecordType的类型
type RecordType = {
 a: string[];
 b: string[];
 c: string[];
}
```

## 索引签名类型

> [!NOTE]
>
> 使用场景：当前无法确定对象中有哪些属性（或者说对象中可以出现任意多个属性），此时就需要用到索引签名类型了

```typescript
interface AnyObject {
    [k: string]: number
}

let obj: AnyObject = {
    a: 1,
    b: 123,
    c: 1234
}
```

索引签名类型一般只能存在一个

```typescript
interface AnyObject<T> {
    [k: string]: number
    [n: number]: T // error
    // 在javascript的对象中，所有的键最终都会转换为字符串 所以此处的键为number类型但实际输入1时，它也匹配string类型，TS会不知道匹配哪一个类型约束
    // 如果实在要使用两个就得让string的索引签名类型也兼容number类型的
    
    [k: string]: number | T
		[n: number]: T
}
```

> [!NOTE]
>
> 数组通过[]索引来访问数组中的元素具体实现就是使用索引签名类型来实现的，具体实现[跳转到实现](#array-index)

### 映射类型

基于旧类型创建新类型（对象类型），减少重复、提升开发效率

例:类型PropKeys中有xyz，Type中也有，并且Type中xyz的类型相同

```typescript
type PropKeys = 'x' | 'y' | 'z'
type Tyep = {
    x: number
    y: number
    z: number
}
```

我们可以简化书写

```typescript
type Type1 = { [Key in PropKeys]: number }

// Type1的类型
type Type1 = {
 x: number;
 y: number;
 z: number;
}
```

> [!WARNING]
>
> 映射类型只能在type中使用，不能在interface中使用

### keyof

除了联合类型创建新类型外，还可以根据对象类型来创建

```typescript
type PropKeys = {
    a: number
    b: string
    c: boolean
}

type Type = { [key in keyof PropKeys]: number }
```

### Partial的实现

Partial的实现也是通过映射类型实现的

[Partial的说明](#partial)

```typescript
type Partial<T> = {
    [K in keyof T]?: T[K]
}
```

### 索引查询类型

语法： T[P]

用来查询属性的类型

```typescript
type Type = {
    x: number
    y: number
    z: number
}

type TypeA = Type['y']
// TypeA 的类型
type TypeA = number

// 或查询多个类型
type TypeB = Type[keyof Type]
```

## 类型声明文件

现如今所有的JavaScript应用都会引入许多第三方库来完成需求。

这些第三方库不管是否是使用TypeScript编写的在最终发布时都必须编译成JavaScript代码才能供开发者使用。

我们知道是TS提供了类型，才有了代码提示和类型保护等机制。

但在项目开发中使用第三方库时，你会发现它们几乎都有相应的TS类型，这些类型都是通过**类型声明文件**来为已存在的Js库提供类型信息

这样在TS项目中使用这些库时，就像用TS一样，都会有代码提示、类型保护等机制。

**类型声明文件的后缀** ：**.d.ts**

在**.d.ts**类型声明文件中，不得编写可执行代码

```typescript
// 类型声明文件

// 类型
type Props = { x: number, y: number }

// 错误演示：.d.ts 文件中，不能出现可执行代码（代码实现）
// 可执行代码
function add(num1: number, num2: number) {
    return num1 + num2
}

console.log(add(1, 5)) // error
```

> [!WARNING]
>
> 在Vue3的配置文件中默认开启 **skipLibCheck** 所以在.d.ts中的可执行代码不会报错
>
> 如需修改
>
> 在 **tsconfig.app.json** 中将
>
> "CompilerOptions" {
>
> ​	"skipLibCheck":false
>
> }
>
> 添加即可

### 使用类型声明文件

#### 库自带的类型声明文件

在所调用的包中使用类型声明文件，只需import导入对饮包即可，ts会自动在对应包中的package.json中找到typings/types字段所对应的声明文件

#### 由definitelyTyped来提供

有些js库本身没有提供ts类型声明文件，可以通过开源项目definitelyTyped来提供该库对应的类型声明文件

先前需要在TypeScript官网进行查找对应库是否有对应的类型声明文件，现如今npm已经内置该功能了。

### 创建自己的类型声明文件

##### index.d.ts

```typescript
// 类型声明文件

// 类型
type Props = { x: number, y: number }

// 模块化语法
export { Props }
```

##### index.ts

```typescript
// 使用模块化语法导入
import type { Props } from './index'

let p1: Props = {
  x: 1,
  y: 2
}
```

### 给已开发完成的js文件提供类型声明

在没有为已完成的js文件提供类型声明时，当这些模块导入到ts文件中，ts将会提示

##### index.ts

```typescript
import {} from './utils.js'
```

**无法找到模块“./utils.js”的声明文件。“/Users/clearsparkling/Project/DocPlatform/src/types/utils.js”隐式拥有 "any" 类型。**

接下来将展示如何为js文件提供类型声明文件

#### declare关键字

用于类型声明，为其他地方（比如.js文件）已存在的变量类型声明，而不是创建一个新的变量

##### utils.js

```typescript
let count = 10
let songName = '冰河时代'
let position = {
    x: 0,
    y: 0
}

function add(x, y) {
    return x + y
}

function changeDirection(direction) {
    console.log(direction)
}

const fomartPoint = point => {
    console.log("当前坐标：", point)
}

export { count, songName, position, add, changeDirection, fomartPoint }
```

##### utils.d.ts

```typescript
declare let count: number
declare let songName: string

interface Point {
    x: number,
    y: number
}
declare let position: Point

declare function add(x: number, y: number): number

declare function changeDirection(direction: Point): void

// 箭头函数的类型声明
// 使用类型别名
// type FomartPoint = (point: Point) => void
// declare const fomartPoint: FomartPoint

// 省略类型别名直接声明
declare const fomartPoint: (point: Point) => void

// 注意：类型提供好以后，需要使用 模块化方案 中提供的
//      模块化语法，来导出声明好的类型。然后，才能在
//      其他的 .ts 文件中使用
export { count, songName, position, add, changeDirection, fomartPoint }
```

这时候在其他ts文件中导入该js模块错误提示就会消失，同时拥有ts类型

## Function类型注解

### 基础函数类型注解

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

### 可选参数

在可传可不传的参数名称后添加 ? （问号）即可声明该参数是可选参数

> [!WARNING]
>
> 可选参数禁止出现在必选参数的前面

## Type Guards - 类型守卫

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

## Typeof操作符

在JS中提供了typeof操作符，用来在JS中获取数据的类型。

```typescript
console.log(typeof "Hello TS") // string
```

在TS中同样提供了typeof操作符，可以在**类型上下文**中引用变量或属性的类型

TS会根据已有变量的值，获取该值的类型，来简化类型书写

```typescript
let point = {
  x: 1,
  y: 2
}

function formatPoint(point:{ x: number, y: number }){}
// 使用TS可以在类型上下文中使用typeof来简化类型书写
function formatPoint(point: typeof point){}

formatPoint(point)
```

> [!WARNING]
>
> typeof只能用来查询变量或属性的类型，无法查询其他形式的类型（例如函数的调用类型，函数的返回值类型）

## Prototype

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

## 手搓系列

### Array

```typescript
interface ArrayType<T> {
    data: T[]
  	// 在接口中无需声明构造器的类型 constructor
    forEach: (callbackFn: (value: T, index: number, array: T[]) => void, thisArg?: any) => void
}

// 这里是由class声明的泛型变量T 然后传递给ArrayType
class NumberArray<T> implements ArrayType<T> {
    data: T[]

    constructor(data: T[]) {
        this.data = data
        for (let index = 0; index < data.length; index++) {
            let value = data[index]
            if (value) {
              	// 索引签名类型赋值
                this[index] = value
            }
        }
    }

    forEach: ArrayType<T>['forEach'] = (callbackFn, thisArg) => {
        for (let index = 0; index < this.data.length; index++) {
            let value = this.data[index]
            if (value !== undefined) {
                callbackFn.call(thisArg, value, index, this.data)
            }
        }
    }

		// 声明索引签名类型
    [index: number]: T
}

const ArrayTest = new NumberArray(['clearsparkling', 'clear', 'spark',1,{}])
ArrayTest.forEach((params,index,array) => {
    console.log(params)
    console.log(index)
    console.log(array)
})
```

