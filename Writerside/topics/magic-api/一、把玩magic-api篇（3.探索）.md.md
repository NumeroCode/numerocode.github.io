# 【把玩magic-api篇】 探索magic-api(接口)
## 预先准备
- 创建数据库test
``` sql
CREATE DATABASE IF NOT EXISTS test;
```

- 创建`商品分类`表
``` sql
CREATE TABLE `category` (
  `id` varchar(32) NOT NULL  COMMENT '主键ID',
	`data_create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '数据创建时间',
  `data_update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP  COMMENT '数据更新时间',
  `data_state` tinyint NOT NULL DEFAULT '1' COMMENT '数据状态 1 有效 2 删除',
	`creator` tinyint NOT NULL  COMMENT '数据创建者',
	`updator` tinyint NOT NULL  COMMENT '数据更新者',
	
	`name` varchar(100) NOT NULL COMMENT '分类名称',
	`code` varchar(20)  NOT NULL COMMENT '分类编码',
  `parent_id` bigint NOT NULL COMMENT '父节点ID',
  `node_path` varchar(500) NOT NULL COMMENT '节点路径',
	`sort` int NOT NULL COMMENT '排序(数值越低优先级越高, 可重复)',
	`enable_state` tinyint NOT NULL DEFAULT '1' COMMENT '数据启用状态 1 启用 2 停用',
  `memo` varchar(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '说明',
  
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB  COMMENT='分类';
```
- 创建`商品`表
``` Sql

CREATE TABLE `goods` (
  `id` varchar(32) NOT NULL  COMMENT '主键ID',
	`data_create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '数据创建时间',
  `data_update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP  COMMENT '数据更新时间',
  `data_state` tinyint NOT NULL DEFAULT '1' COMMENT '数据状态 1 有效 2 删除',
	`creator` tinyint NOT NULL  COMMENT '数据创建者',
	`updator` tinyint NOT NULL  COMMENT '数据更新者',
	
	`category_id` varchar(100) NOT NULL COMMENT '分类ID',
	`type` varchar(10) NOT NULL COMMENT '商品类型(Sigle:单品;Page:套餐)',
	`name` varchar(100) NOT NULL COMMENT '商品名称',
	`code` varchar(20)  NOT NULL COMMENT '分类编码',
	`sort` int NOT NULL COMMENT '排序(数值越低优先级越高, 可重复)',
	`enable_state` tinyint NOT NULL DEFAULT '1' COMMENT '数据启用状态 1 启用 2 停用',
	`photo_example` tinyint NOT NULL DEFAULT '1' COMMENT '图例',
  `memo` varchar(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '说明',
  
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB  COMMENT='商品';
```
## 一 整体布局
![magic-api页面布局](magic-api页面布局.png)

## 二、数据源配置

### 添加数据源(以MySQL为例)

![添加数据源](添加数据源.png)

- 数据源参数数据

| 参数名称 | 参数值示例 | 解释说明 |
| -- | -- | -- |
|名称|商品数据库|数据源的名称，方便人为管理|
|**key**|goods|数据源key, 后续编码中会引用|
|url|jdbc:mysql://<IP>:<PORT>/test|数据源url|
|用户名|root|数据库用户名|
|密码|123456|数据库密码|
|驱动类|com.mysql.cj.jdbc.Driver|数据库驱动类名|
|类型|DruidDataSource|连接池类型|
|maxRows|-1|数据库返回结果的最大行数|
|其它配置|{"useSSL": false, "useUnicode": true, "characterEncoding": "UTF-8", "serverTimezone": "Asia/Shanghai"}|连接池其他配置|

## 三、接口配置

### 接口分组
![创建接口分组](创建接口分组.png)

**友情提示：**
- 分组路径：
  - 路径格式：/分组1/分组2/.../分组n
- 接口分组可以嵌套创建

**推荐分组设计:**

![接口分组设计](https://gitee.com/jinjun/low-code-md/raw/master/magic-api/attachments/%E6%8E%A5%E5%8F%A3%E5%88%86%E7%BB%84%E8%AE%BE%E8%AE%A1.png)
- 基础分组：
  - 路径：/base
  - 说明：基础分组，包含系统的基础数据接口，如：用户、角色、权限、字典等
- 业务分组：
  - 路径：/business
  - 说明：业务分组，包含系统的业务数据接口，如：订单、商品、库存等
- 扩展分组：
  - 路径：/extension
  - 说明：扩展分组，包含系统的扩展数据接口，如：短信、支付、物流等

### 接口
#### Post请求
![alt text](image-7.png)

![alt text](image-8.png)

![alt text](image-9.png)
#### Get请求
![alt text](image-6.png)
![alt text](image-5.png)
![alt text](image-10.png)

#### PUT请求

#### DELETE请求
## 四、函数

## 五、定时任务