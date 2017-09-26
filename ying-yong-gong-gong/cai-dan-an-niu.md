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

### 



