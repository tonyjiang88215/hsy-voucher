# 全键盘

全键盘是指让用户在不使用鼠标的情况下，仅使用键盘即可进行整张单据的录入。

[查看全键盘概念原理](/chapter1/quan-jian-pan/quan-jian-pan-gai-nian-yuan-li.md)

由于单据中使用了大量的参照组件，所以为了支持单据全键盘录入，参照也需要支持全键盘。

## 单据全键盘

单据全键盘主要负责的是外围焦点切换顺序的管理。它会监听用户的键盘输入，并判断当前元素是否可以失焦，并找到下一个可以获取焦点的元素。

当最后一个元素获取焦点时，下一个会跳转到第一个元素，形成一个循环。

## 参照全键盘

参照全键盘主要指的是在使用参照组件录入时，参照组件内部所需要支持的全键盘事件，比如 ReferGridComponent 的高级弹出参照，弹出高级后，这个界面需要自己处理全键盘事件。主要实现了以下特性：

1. Grid 参照 Enter 键打开高级参照；
2. Grid 高级参照支持上下键；
3. Grid 高级参照支持 Enter 键选择值；
4. Grid 高级参照支持 Esc 键关闭高级参照；

### 

## 现成可用组件

单据公共 以及 应用公共已经提供了基本的全键盘组件，可供直接使用

在 `/src/components/voucher/qwert/refer` 目录下的组件，是实现了参照全键盘的组件，但是并未接入单据全键盘管理：

* ReferEditor **等同于 ReferField**;
  * ReferGridEditor **等同于 ReferGridComponent**；
  * ReferCommonEditor **等同于 ReferField - ReferGridComponent**；



在 `/src/components/voucher/qwert` 目录下的组件，是基于参照全键盘基础上，接入了单据全键盘的组件，只能在单据头部或者单据扩展区使用，如果在组件内使用，请使用上边的组件：

* QwertEditor **等同于 DataField**;
  * QwertReferEditor **等同于 ReferField**;
    * QwertReferGridEditor **等同于 ReferGridComponent**；
    * QwertReferCommonEditor **等同于 ReferField - ReferGridComponent**；
  * QwertCommonEditor **等同于 DataField - ReferField**；

### 

## 如何定义自己的元素

#### 简单组件

对于简单组件来说，一般只有一个输入框，获取焦点后，输入完值，按下 Enter 或者 Tab 就直接离开，这样的组件，可以参考 `/src/components/voucher/qwert/QwertCommonEditor.tsx` 来实现；

#### 复杂组件

如果一个组件，内部有多个输入框，组件也有自己的键盘逻辑，那么就需要自己来管理内部的焦点游走事件了，可以参考 `/src/components/voucher/qwert/QwertVoucherGrid.tsx` 来实现；

复杂组件的核心思想就是把组件当做黑箱，只处理边界组件的焦点问题（最上边的组件和最下边的组件），中间部分由组件自己管理；

> QwertVoucherGrid 是对单据表体的封装，由于 Grid 实现了自己的全键盘处理，所以我们只需要处理两种方向的情况：
>
> 1. 焦点从 表头区域 -&gt; 表体区域 -&gt; 扩展区；
> 2. 焦点从 扩展区域 -&gt; 表体区域 -&gt; 表头区域；
>
> **任何复杂组件的全键盘处理，实质都是在处理这两个方向的边界过度问题；**  
> 具体可以参考 QwertVoucherGrid 的实现；



