# PostgreSQL 数据库文档

## 风险数据集市文档
_简要说明数据集市使用方法，数据源以及数据处理方法_


## 数据库连接
连接方法：host=10.202.5.218 port=5432 dbname=risk_datacenter user=postgres password=risk


## Database Schemas

### Schema: [public]
_主要包括了彭博交易记录，汇率，价格信息等_

#### Table: [f3221th1]
**简介**: _彭博交易记录，每个工作日更新,数据来自TSIP生成的交易数据_

**Primary Key**: _ticket_nubmer_

注: _由于交易有修正的可能，当同一个ticket_number的相关交易信息有修改时，在导入数据库时会自动覆盖之前的历史记录，确保这个表里只有一条有效的ticket_number，覆盖的记录可以在表thupdate_log中查看，修改的交易类型在ticket_type_change_log中查看_

#### Table: [bbgtickettype]
**简介**: _彭博交易类型表，包含了大部分的交易类型和对应的含义_

**关系**: _code和f3221th1的tblt_ticket_type相关_

#### Table: [product_subflag]
**简介**: _彭博产品子类型表，包含了大部分的产品类型及其细分类型和对应的含义_

**关系**: _code和f3221th1的tblt_product_subflag相关_


#### Table: [f3221af1]
**简介**: _彭博每日持仓数据。数据来自TSIP生成的持仓数据_


#### Table: [fxrate]
**简介**: _汇率数据_

### Schema: [reports]
_包括报表，结单等文件提取出的数据_

#### Table: [report]
**简介**: _根据基金的托管文件提取出的资产，负债和权益数据。该数据是从MongoDB的risk_datacenter数据库里抓取文件进行解析后得到的数据_


### Schema: [statement]
_资产管理基金的持仓、现金等表，数据来自：\\10.202.7.240\am-ops-rm_

#### Table: [amd_balance]
**简介**: _资产管理基金的持仓_

#### Table: [amd_cash]
**简介**: _资产管理基金的现金_

#### Table: [amd_accounting]
**简介**: _资产管理基金的会计数据_

#### Table: [amd_trader]
**简介**: _资产管理基金的交易数据_

#### Table: [filelog]
**简介**: _资产管理基金的文件记录_

#### Table: [tfifa]
**简介**: _天风国际的财务报表。报表按资产、负债和权益分为三个资产类型，每个类型有各自的会计科目及对应的值_


### Schema: [basic]
_基础数据表，主要包括了产品类型，交易员信息等_


#### Table: [book_remap]
**简介**: _彭博交易账户映射表，包含了交易账户变更信息_

#### Table: [trader]
**简介**: _交易员信息表，包含了交易员的姓名，部门，头衔等信息_


## Change Log
| Date       | Change                | Reason            |
| ---------- | --------------------- | ----------------- |
| 2025-05-23 | 建立文档 | 介绍数据库主要功能 |
