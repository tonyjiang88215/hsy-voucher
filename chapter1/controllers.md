# Controllers

Controller 中包含了单据公共的功能集合，分为以下几部分：

## AuthController {#authcontroller}

功能权限管理器，负责维护当前用户对功能的可见程度，以及当前单据在目前进度下，功能的可用程度；

在 AuthController 中，会根据 transaction\_type 中定义的枚举值为 key ，来保存每一个功能的两种状态值： visible（是否可见） 和 enable（是否可用），其中：

* visible 表示该功能对当前用户是否可见；
* enable 表示该功能对当前用户是否可用（是否可用取决于当前单据的状态，由后端给出）；

## BusinessController {#businesscontroller}

业务管理器，负责单据通用的 CRUD 接口。并且以 Command 模式提供出来支持调用；

支持了 blank（新BO对象）、query（查询单据数据）、save（保存单据）、approve（审核单据）、unapprove（反审单据）等业务接口；

支持了 gotoNew、gotoView、closeAndGotoList、gotoList、gotoSetting 等路由跳转；

## CommandController

命令管理器，负责维护单据所有可执行的命令。目前命令包括了全部的CRUD 和 单据公共的按钮功能，具体可以参考枚举 `CommandTypes`。

## EventController

事件管理器，负责管理单据内事件的注册、监听、派发，不予赘述。

## FomatterController

格式化处理器，负责对字段进行格式化处理，会提供默认的格式化函数，默认格式化会处理枚举、参照、日期这三种特殊字段；

支持具体单据自定义格式化函数；

## FormController

单据数据管理器，单据最核心的控制器，负责创建单据数据实例，管理单据的字段状态，公共的数据逻辑（比如：参照携带字段联动更新，自定义字段联动更新等）。

提供了对数据进行清洗的API：clean、cleanBody、cleanBodyRow等方法；

提供了获取服务端可用数据的API： getValue；

提供了对明细行操作的API： insertRow、insertOrReuseRow、appendRow、appendOrReuseRow；

## GridProfileUpdater

单据表体列配置管理器，当用户修改单据表体的列配置时，将配置信息同步到单据模型中。

## LifecycleController

生命周期管理器，负责维护所有生命周期，并在生命周期执行时，同步触发对应的事件；

## ModuleController

模块加载器，负责维护所有异步加载的模块及实例。

## QwertController

全键盘管理器，根据单据模型生成全键盘焦点顺序，并管理焦点的切换。@qwert 装饰器所装饰的组件，会被注册到QwertController。

## StateController {#statecontroller}

状态管理器，负责管理单据的编辑状态、业务状态 以及 侧边栏的UI状态，持有单据初始化的Option，并将默认的Option进行merge，来保证代码执行的正确性；

## StyleController

样式管理器，全局统一样式在这里进行处理，包括以下：

1. 每个单据实例维护自己的 class-namespace；
2. 单据表体 disable 背景色处理；



