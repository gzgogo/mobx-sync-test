# mobx同步更新验证

今天抽时间验证了一下mob的同步更新问题，感兴趣的朋友可以到github上下载代码： **[mobx-sync-test](https://github.com/gzgogo/mobx-sync-test)**

最终验证的结论如下：
1. 确实为同步更新，每改变一个`observable`变量，都会执行一个`autorun`
2. `React`组件情况特殊，在组件的方法内单独更改每个`observable`变量时，一个函数内的所有更改只会触发组件的一次`render`

结合具体代码：
`increase`与`increaseInReact`两个函数，都是单独设置`user`对象的每个字段，但是效果却截然不同，由React组件触发的方法（`increaseInReact`）只会触发一次`render`，由`dom`元素触发的方法（`increase`）却会触发5次`render`。
       
#### 结论（只是猜测）
react组件触发的方法应该是做了什么特殊处理，保证其一次事件循环内产生的
`render`做批量处理，目前只是猜测，有人知道原因还请指教。

>由于并没有看react和mobx-react的源码，所以结论只是猜测，哪位大神了解细情还请指教。
