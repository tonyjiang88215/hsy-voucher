# 菜单按钮

菜单按钮指的是单据右上方区域的按钮集合组件，菜单按钮通过配置的方式，快速定义出所需要的按钮组件。

菜单按钮支持两级结构。

## 定义

```typescript
// /src/modules/voucher/public/VoucherMenuStateManager.ts

// 按钮定义
export interface IVoucherMenuAction extends IVoucherAction<IVoucherMenuState> {
    // id
    actionId?: string;
    // 状态初始值
    getState(): Promise<IVoucherMenuState> | IVoucherMenuState;
    // 组件
    getComponent?(presenter: VoucherPresenter, state: IVoucherMenuState): Promise<any>;
    // 如果还有子按钮。在这里定义
    children?: Array<IVoucherMenuAction>;
}
```

## 公共按钮

在 /src/modules/voucher/public/actions/menuActions.ts 文件中，定义了公共部分的功能按钮，在具体单据中，只需要直接使用即可

## 按钮状态

单据在不同的状态下，显示的按钮集合不同，按钮的可用状态也不同，这时我们需要对按钮的状态进行管理。

```typescript
// Location: /src/components/voucher/controllers/StateController.ts

// 单据 Action 的状态对象，可以被继承进行状态扩展
export interface IVoucherActionState{
    visible: boolean;
    enable: boolean;
}
```

我们认为公共的 Action 状态都至少需要 可见（visible）、可用（enable）两种状态。

