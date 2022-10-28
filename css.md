# css tips

## 出现滚动条造成页面跳动问题

悬浮滚动条

```css
overflow: overlay;
```

- [overlay](https://segmentfault.com/a/1190000017044563)
- [scrollbar-gutter](https://www.zhangxinxu.com/wordpress/2022/01/css-scrollbar-gutter/)

## 自适应宽度以稳定的子元素为锚定宽度, 防止页面布局变化不稳定

测量子元素中稳定的元素的宽度，作为其它非稳定子元素的固定宽度

```js
// react-use
const [ref, { width }] = useMeasure();
```

## 宽度自适应到自定义宽度的过渡失效

通过设置最大宽度来使过渡生效又不影响自适应宽度

- [参考](https://blog.csdn.net/qq_43942185/article/details/125291441)
- [参考 2](https://sekin.gitbook.io/css-trick/guo-du-dong-hua/gao-du-zi-shi-ying-de-guo-du)
