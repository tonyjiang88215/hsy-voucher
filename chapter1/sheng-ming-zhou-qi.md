# 生命周期

单据生命周期是在单据不同时刻给出的扩展点，允许具体单据在特定时刻进行自己的业务逻辑处理。

```typescript
/**
 *  定义在 /src/components/voucher/controller/LifecycleController.ts 文件中
 */
// 单据生命周期方法集合
export interface IVoucherLifecycle {
    //初始化之前
    beforeInitOption(params?: any) => void;
    //初始化之后
    onInit(params?: any) => void;
    //创建instance之前，data 是 初始化数据
    beforeCreateInstance(data: any) => void;
    //创建instance之后, instance 是 formController.data
    onInstanceCreated(instance: any) => void;
    //开始编辑
    onEditStart(params?: any) => void;
    //结束编辑
    onEditEnd(params?: any) => void;
    //新增行
    onCreateRow(bodyName: string, rowField: VoucherUIField, rowIndex: number, disposers: Array<Lambda>) => void;
    // 批量新增行
    onCreateRowInBatch(batches: Array<{fieldName: string, rowField: VoucherUIField, rowIndex: number, disposers: Array<Lambda>}>) => void;
    //删除行
    onDeleteRow(bodyName: string, rowField: VoucherUIField, rowIndex: number) => void;
    //新增扩展字段（数据）行
    onCreateCustomRow(fieldName: string, rowField: VoucherUIField, rowIndex: number, disposers: Array<Lambda>) => void;
    //删除扩展字段行
    onDeleteCustomRow(fieldName: string, rowField: VoucherUIField, rowIndex: number) => void;
    //保存之前回调，返回true，阻断保存，返回false，执行保存
    beforeSave(data: any) => boolean | Promise<boolean>;
    //保存成功回调
    onSaved(data: any) => void;
    // 删除之前回调，返回true，阻断删除，返回false，执行删除
    beforeDelete(params?: any) => boolean | Promise<boolean>;
    //删除成功回调
    onDeleted(params?: any) => void;
    //页签被激活
    onTabActive() => void;
    //页签被切出
    onTabUnactive() => void;
}
```

这里不对所有生命周期进行说明了，_**大部分生命周期和字面意思一样，这里只对非常重要的生命周期进行说明**_

## onInstanceCreated

当单据的数据需要发生替换，更新，重新查询时，会根据新数据重新生成一个数据实例，此时会触发 onInstanceCreated。

参数 instance 是基于 `simple-form` 创建的实例。

生命周期有两种方式可以实现：

1. 在 Voucher.tsx 中重写对应方法；
2. 通过 EventController 进行对应生命周期的事件监听；

## onCreateRow

当单据子表新增行时，会创建调用当前的生命周期，参数中会把当前子表的字段名，以及 RowField 返回。

> 注： RowField 不要被持有，当 onInstanceCreated 触发时，所有之前的 Field 都会作废。

## beforeSave

当要调用接口保存单据时，会调用这个生命周期，这个生命周期允许 return false 来阻止保存。同时也支持 Promise 格式。

## beforeDelete

当要调用接口删除单据时，会调用这个生命周期，这个生命周期允许 return false 来阻止删除。同时也支持 Promise 格式。

## 如何使用

单据的生命周期提供了两种使用方式：

### 一、在 Voucher 上重写生命周期方法

单据公共提供了 Voucher 组件，用于重写生命周期及UI区域的renderer；

应用公共层面继承了 Voucher，提供了 PublicVoucher，应用公共在单据公共的基础上，增加了各区域通用的组件。

所以具体单据只需要直接继承 PublicVoucher 即可。

> 具体可以参考【销货单】的实现方式：/src/modules/voucher/goods-issue/GoodsIssueEdit.tsx

### 二、通过 EventController 监听生命周期事件

单据公共提供了 EventController 用于派发各个不同的事件，其中包括所有的生命周期。生命周期对应的事件，被定义在：`/src/components/voucher/controllers/EventController.ts` 文件中

```typescript
// 单据生命周期事件，暂时内部使用
export const VoucherLifecycleEvent = {
    // 初始化之前
    beforeInitOption: 'beforeInitOption',
    // 初始化之后
    onInit: 'onInit',
    // 创建instance之前，data 是 初始化数据
    beforeCreateInstance: 'beforeCreateInstance',
    // 创建instance之后, instance 是 formController.data
    onInstanceCreated: "onInstanceCreated",
    // 开始编辑
    onEditStart: 'onEditStart',
    // 结束编辑
    onEditEnd: 'onEditEnd',
    // 新增行
    onCreateRow: 'onCreateRow',
    // 删除行
    onDeleteRow: 'onDeleteRow',
    // 批量新增行
    onCreateRowInBatch: 'onCreateRowInBatch',
    // 新增扩展字段行
    onCreateCustomRow: 'onCreateCustomRow',
    // 删除扩展字段行
    onDeleteCustomRow: 'onDeleteCustomRow',
    // 保存之前回调
    beforeSave: 'beforeSave',
    // 保存成功回调
    onSaved: 'onSaved',
    // 删除之前回调
    beforeDelete: 'beforeDelete',
    // 删除成功回调
    onDeleted: 'onDeleted',
    // 页签被激活
    onTabActive: 'onTabActive',
    // 页签被切出
    onTabUnactive: 'onTabUnactive',
    // 开始编辑某一行
    onRowEditStarted: 'onRowEditStarted',
    // 结束编辑某一行
    onRowEditStopped: 'onRowEditStopped',
    // 销毁前调用
    beforeDestroy: 'beforeDestroy'
}
```

_**注意：通过事件方式监听生命周期，不能阻止生命周期的继续执行，也就是说 beforeSave 和 beforeDelete 不能通过 return false 来进行阻止；**_

