#### Four types of tweens:

``` typescript
gasp.to()
// 最常用的 补间动画会从元素的当前位置开始，并逐步动画到补间动画定义的值
gasp.from()
// 同.to()类似 它从补间动画定义的位置逐步动画到元素的当前状态
gasp.fromTo()
// 需要同时定义动画的起始值和结束值
gasp.set()
// Immediately sets properties (no animation). It's essentially a zero-duration .to() tween.
// 本质上是一个无补间动画的.to()
```

#### The target(or targets)

```typescript
// 使用ID or Class选择器
gsap.to(".box",{
    x:200
})
gsap.to("#box",{
    x:200
})

// 甚至是复杂的css选择器
gsap.to("section > .box",{
    x:200
})

// 甚至是一个变量（需要是HTMLElement类型）
let box = document.querySelector(".box")
gsap.to("box",{
    x:200
})

// 还可以是一个包含多个变量的数组（同上 依旧需要是HTMLElement类型）
let square = document.querySelector(".square");
let circle = document.querySelector(".circle");
gsap.to([square, circle], {
    x: 200 
})
```

#### Transform shorthand

转换简写方式/Value值

```typescript
gsap.to("value",{
    x:value,
    y:value,
    // 百分比
    xPercent:value,
    yPercent:value,
    // 缩放
    scale:value,
    scaleX:value,
    scaleY:value,
    // 旋转
    rotation:value,
    rotation:"value",
    // 偏移
    skew:value,
    skewX:value,
    skewY:value,
    // 变形原点
    transformOrigin:"value",
    //
    opacity:value
    
})
```

