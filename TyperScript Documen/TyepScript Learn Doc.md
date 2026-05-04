# TyepScript Learn Doc

## Function类型注解

``` typescript
// 参数类型注解
function TypeTest(value: string) {

}

// 返回值类型注解
function TypeReturnTest(): string {
    return 'Test'
}

// 箭头函数声明的方法使用类型注解
const TypeTest = (value : string):string => {
    return "Test"
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

