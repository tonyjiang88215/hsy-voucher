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



