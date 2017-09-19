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

## StateController {#statecontroller}

状态管理器，负责管理单据的编辑状态、业务状态 以及 侧边栏的UI状态，持有单据初始化的Option，并将默认的Option进行merge，来保证代码执行的正确性；

