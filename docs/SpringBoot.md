# SpringBoot理财金融平台

|用户管理|账户管理|支付管理|产品管理|风险管理|法规、资质|


## 要求
  * 快速 - 开发快、迭代快
  * 安全 - 加密、权限控制
  * 高效 - 高并发、响应快

  ### 快速
  JUnit自动化测试，Swagger文档工具
  * 销售端
  * 管理端

  ### 高效
  JSON-RPC 良好的数据库设计 hazelcast缓存

  ### 安全
  HTTPS RSA签名 权限控制 节流限速 访问统计 tyk

## 前置知识
* Spring和Java Web相关基础
* gradle构建项目
* Spring Boot基础

## 模块设计
* 通用模块
  * Util 工具
  * Quartz 定时任务
  * Swagger 接口管理
* 本项目公用的模块
  * Entity 实体
  * Api 公共接口
* 项目模块

## 技术要点
* 销售端
  * jsonRpc 系统之间的调用
  * hazelcast 缓存
  * activemq 消息队列
  * 自动化测试
  * tyk 接口网关
  * quartz 定时任务


## 缓存
* meecache 最早 结构单一 k/v 
* redis 数据结构多、持久化
* hazelcast 数据结构更多、功能更多、集群更方便、管理界面更好、spring整合更方便
