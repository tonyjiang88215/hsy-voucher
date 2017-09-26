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

## 按钮状态定义

单据在不同的状态下，显示的按钮集合不同，按钮的可用状态也不同，这时我们需要对按钮的状态进行管理。

```typescript
// Location: /src/components/voucher/controllers/StateController.ts

// 单据 Action 的状态对象，可以被继承进行状态扩展
export interface IVoucherActionState{
    visible: boolean;
    enable: boolean;
}
```

我们认为公共的 Action 状态都至少需要 **可见（visible）**、**可用（enable）**两种状态。菜单按钮可以对这个接口进行继承扩展自己的额外状态。

## 按钮状态管理器

由于菜单按钮的状态取决于多个维度共同控制，通常来说，我们只需要对 **visible** 和 **enable** 两种状态进行管理，这两种状态最多由三部分进行控制：**后端权限控制、功能模块控制、具体单据业务控制。**

### 后端权限控制

是指根据用户身份不同 和 单据后端业务状态不同，导致的按钮状态不同。

比如：无权限的用户不能显示审核按钮，已经审核通过的单据不能显示生单按钮 等。

### 功能模块控制

是指当一个或多个按钮完成的功能，根据当前的运行状态，决定按钮状态。

比如：上一张、下一张，当到达第一张 或者 最后一张时，对应的按钮不可点击。

### 具体单据业务控制

是指具体单据根据业务需求，设置自己的按钮状态，

比如：销货单中不想显示打印按钮；

### VoucherMenuStateManager

所以我们使用 VoucherMenuStateManager 来维护菜单按钮状态。VoucherMenuStateManager 会根据按钮关联的控制组合，自行计算当前按钮的状态，并且提供 observable 的状态对象，进行控制。

> 详情参考 /src/modules/voucher/publicVoucherMenuStateManager.ts



