# 全键盘概念原理

![](/assets/单据公共全键盘顺序.png)

如上图所示，整张图描述的就是单据范围内，全键盘大致整体流程。

我们用颜色标明了两种全键盘的顺序，希望能够让你更好的了解全键盘是如何按顺序“ 拼接 ” 起来的。

## 顺序很重要

对用户来说，他只需要不断重复 【录值 -&gt; 回车】，就应该可以完整录入整张单据，所以我们的第一件事就是要定义整张单据的焦点顺序。

从图中我们看到两种颜色，其中，蓝色描述的就是单据焦点顺序。单据的焦点顺序是标准的**从左到右，从上到下**的顺序；

其中，**单据表头区域 和 单据表体区域**的顺序由单据公共自动生成，所以**具体单据只需要自己定义表尾的顺序即可**。

## 全键盘是 "拼" 出来的

从图中我们会发现，蓝色的 6 和 7 之间，有很多红色的箭头。红色的箭头表示的是单据表体自己的全键盘。

当**蓝色6**获取焦点后，这是键盘事件已经交给 Grid 来进行处理，**在 Grid 全键盘没有完成之前，单据不会响应键盘事件**来继续又走到下一个元素**蓝色7。**

只到 Grid 走到最后一个单元格 红色13 时，单据才会恢复响应键盘事件，这样下一次键盘事件才会让焦点游走到 蓝色7.

红色部分的键盘流转，对于整个单据来说，并不感知，单据只关注当前元素是否可以失焦，下一个元素是否可以获取焦点。这件事是由**IQwertItem**来描述的。

### 全键盘元素 IQwertItem

在单据中，可以获取焦点的元素，叫做**全键盘元素（QwertItem）**， 你可以在`/src/components/voucher/qwert/decorators/qwert.tsx`中找到该接口的定义：

```typescript
export interface IQwertItem {
    qwertId: string;

    canActive(): boolean;
    canDeactive(direction: IQwertDirection, keyCode: number): boolean;
    active(direction: IQwertDirection): any;
    deactive(): any;
}
```

* qwertId 标识当前元素的id，唯一标识；
* canActive 当前一个元素准备失焦时，会询问当前元素是否可以获取焦点，如果不能，则跳过当前元素，继续向下寻找；
* canDeactive 当前元素准备失焦时，询问当前元素是否可以失焦，如果不行，则不做任何操作；
* active 当前元素获取焦点时，被调用的方法，direaction 表示当前移动的方向，keyCode 表示当前按下的键盘；
* deactive 当前元素失去焦点时，被调用的方法；

### 装饰器 @qwert

支持全键盘的组件需要使用 @qwert 装饰器进行包装，并实现 IQwertItem 的接口。具体可以参考 `/src/component/voucher/qwert/` 目录下，Qwert 开头的组件。

### 装饰器 @qwertSelect

qwertSelect 是为了解决在 Grid 中使用 Select 组件 而提供的装饰器，它主要解决了当 Select 显示下拉列表时，阻止上下键，导致 Grid 单元格切换焦点的问题；



**至此，我们已经拥有了可以完成任何全键盘组件所需要的所有概念了。**

