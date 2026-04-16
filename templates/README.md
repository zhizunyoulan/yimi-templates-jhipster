# 模板转换说明

本仓库提供了针对 **Yimi CRUD Generator** IDEA 插件的 Velocity 模板转换方案，源自 JHipster CRUD 生成器的 EJS 模板。当前目的是让使用者理解本套模板的整体思路、已做的简化处理、以及仍需继续完善的地方。

## 目的与范围

本套模板的设计目标是：

- 将 JHipster 的 EJS 生成器逻辑迁移为 Velocity 模板
- 保留核心 CRUD 生成流程
- 兼容 Yimi CRUD Generator 的配置与条件生成机制
- 提供可读的初始化逻辑和注释，方便后续维护

当前实现主要聚焦于典型实体 CRUD 生成场景，尚未覆盖所有 JHipster 特性和所有环境变量。

## 目录与文件说明

- `config.xml`
  - 定义模板映射规则
  - 将源模板文件 (`sourcePath`) 映射到生成目标文件 (`targetPath`)
  - 支持条件表达式 `condition` 和模板分组 `profile`

- `init.vtl`
  - 作为统一入口，构建所有后续模板共用的上下文变量
  - 将原始 JHipster EJS 逻辑转换为 Velocity 语法
  - 处理实体字段、关系、主键、分页、搜索、DTO、服务层、i18n 等通用配置

- `backend/`, `spring-boot/`, `client/_vue/`
  - 存放实际的源模板文件
  - 模板内容仍保留 JHipster 原始逻辑，但切换为 Velocity 写法

## 详细转换思路

### 1. 统一构建上下文变量

`init.vtl` 的核心任务是将复杂的原始逻辑一次性计算出来，后续模板通过引用这些变量来生成代码。其主要工作包括：

- `global` 配置参数映射
  - 例如：`global.searchEngine`、`global.reactive`、`global.service`、`global.entitySpringPreAuthorizePrefix`
- 主键信息提取
  - 识别主键类型、字段名、getter/setter 名称
  - 支持 `UUID` 和 `String` 类型主键的特殊标记
- 字段属性派生
  - 计算 `fieldType`、`fieldNameHumanized`
  - 判断是否包含二进制/时间字段，生成 `anyFieldIsBlobDerived`、`anyFieldIsTimeDerived`
- 关系结构构造
  - 解析 `nestInfo` 的一对一、一对多、多对一、多对多关系
  - 生成 `relationships`、`relationshipsByOtherEntity`
  - 处理 owner/owned side、bag relationship、eager loading 逻辑
- DTO/服务层配置
  - 识别 `resultModelClass.classType == 'DTO'` 的 MapStruct/DTO 模式
  - 计算 `serviceNo`、`serviceImpl` 等服务层生成标志
- 数据库与 Spring Data 模式
  - 根据 `global.databaseType` 和 `global.reactive` 计算 `springDataDescription`
- 前端路径与 i18n
  - 生成 `entityFileName`、`entityFolderName`、`entityTranslationKey`
  - 简化原始 JHipster 的 AngularJS 后缀和路径规范逻辑

这种“先计算、再引用”方式，使得单个模板文件更加简洁，避免在每个模板中重复解析复杂逻辑。

### 2. 条件生成与插件兼容

`config.xml` 中的模板映射是 Yimi 插件的核心入口。

- 通过 `condition` 可以控制某些文件仅在特定场景下生成，例如：
  - 只读模式不生成 update 页面
  - `global.reactive` 决定生成 Reactive repository
  - `resultModelClass.classType == 'DTO'` 决定生成 DTO/Mapper
- 前端模板划分为 `admin` profile，目标目录为 `src\main\webapp`
- 后端模板同时支持标准 JPA、Reactive、R2DBC、Elasticsearch 等不同变体

这意味着你可以继续在 `config.xml` 中补充更多条件分支，适配更多插件参数。

### 3. 已做的简化与处理方式

为了降低迁移复杂度，本次转换对原始 JHipster 逻辑进行了有意识的简化：

- `generator.embedded` 固定为 `false`
  - 当前不处理嵌入式实体、@Embeddable 等复杂结构
- `entityPersistenceLayer` 固定为 `true`
  - 后端持久层总是按照常规 JPA/Repository 生成
- AngularJS 后缀与历史路径逻辑被简化
  - 例如：`entityAngularJSSuffix`、`normalizePathEnd`、`entityStateName` 等不再单独计算
- 微服务路径简化为 `services/${global.microserviceName.toLowerCase()}/`
  - 仅保留常见的微服务前缀结构
- i18n 前缀统一由 `global.appName` + `entityFileName` 组合形成
- 某些历史变量和分支逻辑未完全保留
  - 例如：旧版 JHipster 的 `microserviceAppName` 处理、AngularJS 特殊后缀、复杂表单验证分支

这些简化经过权衡后，主要保留“最常用的 CRUD 生成路径”，避免过度还原原始框架的所有历史兼容细节。

## 当前支持与已接入变量

当前模板在 `init.vtl` 中已经接入并处理的环境变量包括：

- `global.clientRootFolder`
- `readDefinition.paginationInfiniteScroll`
- `deleteDefinition`
- `createDefinition`、`updateDefinition`、`deleteDefinition`
- `requestModelClass` / `resultModelClass`
- `currentModel.fields`
- `global.reactive`
- `global.entityR2dbcRepository`
- `global.searchEngine`
- `global.service`
- `global.entitySpringPreAuthorizePrefix`
- `global.databaseType`
- `global.applicationType`
- `global.authenticationType`

其中，`init.vtl` 将这些变量标准化为易用上下文变量，供后续模板一致引用。

## 如何继续完善

如果你要继续调整模板，建议从以下方向着手：

1. 补全嵌入式实体支持
   - 目前 `embedded` 固定为 `false`
   - 可以进一步实现 `@Embeddable`、嵌套字段的生成逻辑
2. 完善 Angular 前端路由与文件名规则
   - 恢复或改造 `entityAngularJSSuffix` 和 `entityStateName` 的原始逻辑
3. 扩展 `config.xml` 条件表达式
   - 为更多场景添加 `profile` 或 `condition`
   - 例如：不同前端框架、管理员页面、权限控制等
4. 验证更多 JHipster 生成场景
   - 多对多关系、复合主键、DTO + criteria 查询、Elasticsearch、Reactive 模式
5. 检查插件实际环境变量集合
   - 根据 Yimi CRUD Generator 最新版本，确认 `global`、`currentModel`、`readDefinition` 的完整字段

## AI 协助说明

本套模板的转换过程中，引入了 AI 工具协助分析和整理：

- 帮助理解 JHipster 原始 EJS 逻辑
- 生成 `init.vtl` 的变量派生与注释说明
- 汇总转换思路、简化策略和可维护建议

因此，目前这套模板仍属于“半自动迁移”阶段。它为常见 CRUD 场景提供了基础支持，但可能需要人工校对、测试和继续改进。

---

本文件旨在帮助开发者快速理解转换思路与当前实现边界，并为后续模板维护提供清晰参考。
