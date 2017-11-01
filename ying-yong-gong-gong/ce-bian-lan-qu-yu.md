# 侧边栏区域

侧边栏使用了好生意中的 Slider 组件，并按照单据的规范，生成了新组件 VoucherSlider。

侧边栏包括两个区域：

* 侧边栏按钮
* 侧边栏内容

## 侧边栏按钮

侧边栏按钮是显示侧边栏功能集合的悬浮按钮集合。每一个侧边栏内容都会对应一个按钮，并且在点击该按钮时，进行内容切换。

![](/assets/单据公共-侧边栏.png)

侧边栏按钮区域分为两部分：

* 上部分为单据公共按钮区域，单据共有的功能，按钮集中放于此处。
* 下部分为具体单据按钮区域，每个单据有自己不同的功能，特殊功能集中放于此处。
* 还有一个固定按钮为子表全屏，放在最下方，独立放置。

## 自定义侧边栏

### 定义

```typescript
// 侧边栏 Action 状态对象
export interface IVoucherSliderActionState extends IVoucherActionState {
    // 按钮hover后，展开显示的文字
    label: string;
    // 按钮的图标
    icon: string;
    // 内容区中的表体
    title?: string;
    // 内容是否显示
    isContentShow: boolean;
    // 小红点提示数字（暂未实现）
    badge?: number;
    // z轴高度
    zIndex?: number;
    // 是否显示在第一区域（上边）
    primary?: boolean;
    // 点击图标时的 click
    onClick?: () => void;
}

// 单据 Slider Action
export interface IVoucherSliderAction extends IVoucherAction<IVoucherSliderActionState> {
    // 获取侧边栏状态
    getState(): IVoucherSliderActionState;
    // 获取侧边栏内容组件
    getComponent?(presenter: VoucherPresenter, callback: (instance: any) => void): any;
}
```



