## 在浏览器中的观察器 API

### 什么是浏览器的观察器

在浏览器中，有一些不是由用户直接触发的事件，比如 DOM 元素从不可见到可见、DOM 大小、属性的改变和子节点个数的修改等,
浏览器提供特定的 api 去监控。
浏览器的观察器共有 5 种

- IntersectionObserver(交叉观察器)
- MutationObserver(变化观察器)
- ResizeObserver(大小观察器)
- PerformanceObserver(性能观察器)
- ReportingObserver(报告观察器)

### IntersectionObserver(交叉观察器)

交叉观察器是 "自动" 观察目标元素与根元素的交叉区域的变化。默认根元素为文档视口，此时交叉区域的变化决定了用户在当前视口能否看到目标元素，所以常被用于 "元素可见性" 观察。

```js
const option = {
  // root: document, // 指定根元素，必须是目标元素的父元素，默认是 document
  // rootMargin: "0px 0px 0px 0px", // 因为浏览器盒模型的问题,margin 不会计算在盒子的宽高内，需要该参数补正
  // threshold: 0.0 // 可以是一个 0.0 到 1.0 之间的值或数组，当 0.0 时，目标元素与根元素有1px交叉也会被视为可见，若指定为 1.0，则目标元素需要全部显示才会被视为可见
};
const callback = (entries, observer) => {
  // observer 监听目标元素的实例 observer === InterSelection
  // entries 目标元素数组，通过 InterSelection.observe 按顺序添加进去
  entries.forEach((entry) => {
    // entry 目标元素
    // entry.time 可见性发生变化的时间，是一个高精度时间戳
    // entry.target 目标元素，是一个 dom 对象
    // entry.isIntersecting 可视状态 true 可视 false 不可视
    // entry.intersectionRatio 根和目标元素的交叉区域的比例值
    // entry.boundingClientRect：目标元素的矩形区域的信息
    // entry.intersectionRect：目标元素与视口（或根元素）的交叉区域的信息
    // entry.rootBounds 根元素的矩形区域信息 getBoundingClientRect() 返回值 如果没有根元素，则返回 null
  });
};
const InterSelection = new IntersectionObserver(callback, option);
```

#### InterSelection 参数

- InterSelection.observe(dom) 监听一个元素，如果需要监听多个元素则执行多次

- InterSelection.disconnect() 终止对所有目标元素可见变化的观察

- InterSelection.takeRecords() 返回一个监听对象数组，包含目标元素的监听信息

- InterSelection.unobserve(dom) 停止一个元素的观察

#### 示例

```js
const options = {
  threshold: 0.0,
};
const InterSelection = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    document.querySelector(".isshow").innerHTML = entry.isIntersecting
      ? "显示"
      : "隐藏";
  });
}, options);
InterSelection.observe(document.querySelector(".test1"));
```

### MutationObserver(变化观察器)

该观察器观察目标元素属性和子节点的变化。目标元素 DOM 发生变动就会触发观察器的回调函数。
> 注意：这是异步触发，需要等到当前 DOM 操作都结束才触发，也就是事件循环后触发
