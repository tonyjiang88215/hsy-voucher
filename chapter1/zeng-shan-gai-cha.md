# 增删改查（CRUD）

单据的增删改查分为两部分，一部分是 node 层的接口实现，另一部分是前端的业务实现。

## Node接口

单据在 node 层提供了统一的 增删改查（CRUD）接口，来满足通用性的需求。通常情况下，现有的接口已经可以满足通用需求。

#### 如何重写方法

下面以销售订单为例，展示如何重写销售订单的 update 方法。

一、在 `app-server/service/voucher/`  文件夹中，新增文件 `sales-order.ts`

```typescript
import VoucherBase from './base';

// 以销售订单为例，重写 update 方法。需要继承 VoucherBase 方法
export class VoucherSalesOrder extends VoucherBase {
    async update(entityName: string, id: number, data: any, params: any) {
          console.log('customized update method');
        return super.update(entityName, id, data, params);
    }
}
```

二、在 `app-server/service/voucher/provider.ts` 中进行编辑:

```typescript
// 引入刚刚定义的 VoucherSalesOrder 文件
import VoucherSalesOrder from "./sales-roder";

export default class VoucherService extends AppService implements IVoucherService {

    private serviceClassMap: { [entityName: string]: typeof VoucherBase } = {
          // 在这里定义销售订单的 Class
        ["SalesOrder"]: VoucherSalesOrder
    };

      ...
}
```

三、重启服务即可



## 前端业务

单据公共在前端提供了统一的业务处理，使得具体单据无需进行 CRUD 的接口调用，只需要通过生命周期来进行控制即可。

#### BusinessController

位置： /src/components/voucher/controller/BusinessController.ts 

businessController 主要集中处理了跟 node 相关的接口处理，并以 Command 的模式提供出来，允许使用者调用。





