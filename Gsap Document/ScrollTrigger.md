## ScrollTrigger Value Document

#### start end

Gsap.to Start End 位置

```ts
start:"top center",
end:"bottom top"
```

First Value:与被触发元素的位置相关

Second Value:与Scroll位置相关

Value: 

top center bottom 

Pixels(像素) Percentages(百分比)(Relative to top)

+=Vlaue(被触发元素Start to End的距离)可以为参数

```ts
end:"+=" + () => document.querySelector(".classValue").Vlaue
```



#### markers

Start End 标记显示

```ts
markers:true
```

Value: True False

标记会使Trigger在切换Router页面之后仍保留

解除标记恢复正常

#### snap

对齐值

可以为对象设置参数

sanpTo：对齐的位置

