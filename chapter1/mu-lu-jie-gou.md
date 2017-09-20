# 目录结构

为了后边能够更好的理解单据公共的功能，所以先介绍一下单据公共的目录结构

```
/src/components/voucher
 |---- aop            AOP框架
 |---- commands       单据对象
 |---- controllers    控制器
 |       |---- AuthController       权限控制器，保存功能权限及可用状态
 |       |---- BusinessController   业务逻辑控制器，处理公共接口CRUD
 |       |---- CommandController    命令控制器，负责管理并执行命令
 |       |---- EventController      事件控制器，负责注册、派发事件；
 |       |---- FormatterController  格式化控制器，负责对数据进行格式化处理；
 |       |---- FormController       单据数据管理器，负责维护单据的数据对象，是最核心的管理器；
 |       |---- GridProfileUpdater   子表列配置管理器，负责更新租户级子表配置，同步到单据模型中；
 |       |---- LifecycleController  生命周期控制器，通过AOP注册所有生命周期，并派发生命周期事件；
 |       |---- ModuleController     模块管理器，负责管理异步加载的模块，已经对模块实例的注册；
 |       |---- QwertController      全键盘管理器，负责管理单据全键盘焦点顺序，监听事件并进行管理；
 |       |---- StateController      状态管理器，负责管理单据的状态 和 参数option；
 |       |---- StyleController      全局样式管理器，负责管理单据实例的全局样式；
 |---- Form           单据Form
 |       |---- VoucherFormHelper    基于 Simple-Form 创建 formModelOption 的生成对象；
 |       |---- VoucherUIField       继承 Simple-Form 中 Field，提供给单据所使用的 Field 对象；
 |---- formula        公式计算
 |---- module         功能模块
 |       |---- attachments          附件模块；
 |       |---- dataCheck            数据监测；
 |       |---- draft                草稿；
 |       |---- navigator            单据导航（上一张、下一张）；
 |---- presenter      业务管理对象
 |       |---- VoucherAPI           单据API对象；
 |       |---- VoucherPresenter     单据控制器，持有单据所有的Controllers实例；
 |---- qwert          全键盘
 |---- receiver       命令接受对象
 |---- ui             单据公共的UI组件
```



