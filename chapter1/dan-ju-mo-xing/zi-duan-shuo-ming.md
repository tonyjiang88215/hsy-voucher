# 字段说明

不同的单据模型中，有一部分公共字段是所有单据都需要拥有的，这里列出这些字段，方便在开发新单据时，作为参考：

#### 

#### 单据主体字段

| 字段名称 | 字段含义 | 字段类型 | 可选值 | 是否必填 |
| :--- | :--- | :--- | :--- | :--- |
| id | 自增id | number | - | 是 |
| code | 单据的编码 | string | - | 是 |
| redBlueFlagEnum | 单据的红蓝标志 | string | 枚举 RedBlueFlagEnum | 是 |

#### 

#### 单据明细字段

| 字段名称 | 字段含义 | 字段类型 | 可选值 | 是否必填 |
| :--- | :--- | :--- | :--- | :--- |
| id | 自增id | number | - | 是 |
| sequenceNum | 描述单据明细行排序的字段 | number | - | 是 |
| editFlag | 描述字段的变更状态 | string | old、new、update、delete | 是 |

#### 

#### 单据明细中商品相关字段

| 字段名称 | 字段含义 | 字段类型 | 可选值 | 是否必填 |
| :--- | :--- | :--- | :--- | :--- |
| productId | 商品id | number | - | 是 |
| itemBarcode | 由商品和单据确定的编码 | string | - | 是 |

#### 


