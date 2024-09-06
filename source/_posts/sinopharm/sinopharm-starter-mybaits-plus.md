---
title: Sinopharm Starter  sinopharm-starter-mybaits-plus
rightbar:
  - toc
date: 2024-09-06 10:50:01
tags: sinopharm-starter
---
# 数据访问模块：MyBatis-Plus
## 简介
sinopharm-starter-data-mybatis-plus 是 Sinopharm Starter 数据访问模块针对 MyBatis-Plus 框架的默认处理。
``` xml 
<dependency>
  <groupId>com.sinopharm</groupId>
  <artifactId>sinopharm-starter-data-mybatis-plus</artifactId>
  <version>${version}</version>
</dependency>
```
## 主要特性
- 版本锁定：涉及依赖已进行版本锁定，使用时无需配置版本
- 默认配置：已配置分页插件（启用溢出处理）、防全表更新与删除插件、乐观锁插件、MyBatis 默认配置（自动驼峰命名规则、日志配置）、SQL 打印及性能分析配置（p6spy）
- Mapper 扫描包配置：只需要在配置文件中指定 Mapper 接口扫描包配置
- 扩展 BaseMapper

## 配置示例
配置详情请查看：com.sinopharm.starter.data.mybatis.plus.autoconfigure.MyBatisPlusExtensionProperties
``` yml
--- ### MyBatis Plus 配置
mybatis-plus:
  ## 扩展配置
  extension:
    enabled: true
    # Mapper 接口扫描包配置
    mapper-package: ${project.base-package}.**.mapper
    # 分页插件配置
    pagination:
      enabled: true
      db-type: MYSQL
    # 防全表更新与删除插件(默认开启)
    blockAttackPluginEnabled: false 
    # 乐观锁插件(默认开启)
    optimisticLockerPluginEnabled: false 
```
下方 MyBatis Plus 配置已经在本模块中进行了默认配置，使用者无需再进行配置，如需要更改，可进行覆盖配置。
``` yml
mybatis-plus:
  # 启动时是否检查 MyBatis XML 文件的存在（默认：false 不检查）
  check-config-location: true
  ## MyBatis 原生支持配置
  configuration:
    # 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN（下划线命名）到经典 Java 属性名 aColumn（驼峰命名）的类似映射
    map-underscore-to-camel-case: true
    # MyBatis 自动映射时未知列或未知属性处理策略，通过该配置可指定 MyBatis 在自动映射过程中遇到未知列或者未知属性时如何处理
    auto-mapping-unknown-column-behavior: NONE
    # 日志配置（关闭，单纯使用 p6spy 分析）
    log-impl: org.apache.ibatis.logging.nologging.NoLoggingImpl
```

## 核心依赖
依赖     | 描述
-------- | -----
mybatis-plus-boot-starter  | MyBatis Plus（MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，简化开发、提高效率）
dynamic-datasource-spring-boot-starter  | 	Dynamic Datasource（基于 Spring Boot 的快速集成多数据源的启动器）
p6spy  | P6Spy（SQL 性能分析组件）
sinopharm-starter-core | Sinopharm Starter 核心包
