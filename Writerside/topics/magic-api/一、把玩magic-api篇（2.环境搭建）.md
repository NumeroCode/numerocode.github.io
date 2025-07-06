# 【把玩magic-api篇】 搭建开发环境

## 一、环境准备

### 1. 安装 idea

### 2. 安装 mysql

## 二. 下载并引入到开发环境

```
https://gitee.com/ssssssss-team/magic-api-example.git
```

## 三、执行 建表语句

### 配置表
```sql
CREATE TABLE `magic_api_file_v2` (
  `file_path` varchar(512) NOT NULL COMMENT '配置路径',
  `file_content` mediumtext  COMMENT '配置详情',
  PRIMARY KEY (`file_path`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='配置表';
```
###  备份表
```sql
CREATE TABLE `magic_api_backup_v2` (
  `id` varchar(32) NOT NULL COMMENT '原对象ID',
  `create_date` bigint NOT NULL COMMENT '备份时间',
  `tag` varchar(32) DEFAULT NULL COMMENT '标签',
  `type` varchar(32) DEFAULT NULL COMMENT '类型',
  `name` varchar(64) DEFAULT NULL COMMENT '原名称',
  `content` blob COMMENT '备份内容',
  `create_by` varchar(64) DEFAULT NULL COMMENT '操作人',
  PRIMARY KEY (`id`,`create_date`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='备份表';
```

## 四、修改配置

```
magic-api:
  web: /magic/web # UI请求的界面以及UI服务地址
  resource:
    type: database  # 配置接口存储方式，这里选择存在数据库中
    table-name: magic_api_file  # 数据库中的表名
    prefix: /magic-api  # 前缀
  prefix: / # 接口前缀，可以不配置
  auto-import-module: db  # 自动导入的模块
  auto-import-package: java.lang.*,java.util.* #自动导包
  allow-override: false #禁止覆盖应用接口
  sql-column-case: camel #启用驼峰命名转换
  support-cross-domain: true # 跨域支持，默认开启
  show-sql: true #配置打印SQL
  date-pattern: # 配置请求参数支持的日期格式
    - yyyy-MM-dd
    - yyyy-MM-dd HH:mm:ss
    - yyyyMMddHHmmss
    - yyyyMMdd
  response: |- #配置JSON格式，格式为magic-script中的表达式
    {
      code: code,
      message: message,
      data,
      timestamp,
      requestTime,
      executeTime,
    }
  response-code:
    success: 1 #执行成功的code值
    invalid: 0 #参数验证未通过的code值
    exception: -1 #执行出现异常的code值
  banner: true # 打印banner
  thread-pool-executor-size: 8 # async语句的线程池大小
  throw-exception: false #执行出错时是否抛出异常
  backup: #备份相关配置
    enable: true #是否启用
    max-history: -1 #备份保留天数，-1为永久保留
#    datasource: subject-construction  #指定数据源（单数据源时无需配置，多数据源时默认使用主数据源，如果存在其他数据源中需要指定。）
    table-name: magic_api_backup #使用数据库存储备份时的表名
  crud: # CRUD相关配置
    logic-delete-column: deleted #逻辑删除列
    logic-delete-value: 1 #逻辑删除值
  cache: # 缓存相关配置
    capacity: 10000 #缓存容量
    ttl: -1 # 永不过期
    enable: true # 启用缓存
  page:
    size: size # 页大小的参数名称
    page: page # 页码的参数名称
    default-page: 1 # 未传页码时的默认首页
    default-size: 10 # 未传页大小时的默认页大小
  security:  # 安全配置
    username: admin # 登录用的用户名
    password: 123456 # 登录用的密码
  swagger:
    version: 1.0
    description: MagicAPI 接口信息
    title: MagicAPI Swagger Docs
    name: MagicAPI 接口
    location: /v2/api-docs/magic-api/swagger2.json
  debug:
    timeout: 60 # 断点超时时间，默认60s
```

## 五、Debug启动服务
![alt text](image.png)

服务启动成功，magic-api已内置启动! Access URL

## 六、访问

```
http://<IP>:9999/magic/web/index.html
```

![alt text](image-1.png)

![alt text](image-11.png)