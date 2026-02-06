
# JHipster 模板 for Yimi Crud Generator

## 概述

这是一套专为 **Yimi Crud Generator** 代码生成插件设计的 JHipster 配套模板文件集。

## 核心用途

这些模板能让您在 **Yimi Crud Generator** 中生成符合 JHipster 项目规范、约定和最佳实践的代码，包括标准的 REST 控制器、Service 层、Repository 层及相关配置。

## 🚀 快速开始

### 前置条件
- 已安装 **Yimi Crud Generator** IntelliJ IDEA 插件
- 已有一个基于 JHipster 创建的 Spring Boot 项目

### 关键配置
**重要**：在使用本模板集前，**必须在插件设置中将框架类型设置为 `JPA`**，而非默认的 MyBatis。

### 使用方法
1. 克隆或下载本仓库到本地
2. 将本仓库中的 `templates` 文件夹**复制到您项目的根目录**
3. 在 **Yimi Crud Generator** 插件中配置使用这些模板
4. 选择您的 JPA Entity 类开始生成符合 JHipster 规范的代码

## 📁 模板内容

本仓库包含以下针对 JHipster 的模板：

```
templates/
├── backend/
│   └── jhipster/                 # JHipster 后端模板
│       ├── src/main/java/__base_package__/
│       │   ├── controller/      # REST 控制器 (遵循 JHipster RESTful 约定)
│       │   ├── service/         # 服务层 (使用构造函数注入)
│       │   └── repository/      # Spring Data JPA 仓库接口
│       └── src/main/resources/
│           └── config/          # 配置文件模板
└── frontend/
    └── jhipster/
        └── vue/                 # Vue.js 前端模板
            ├── src/apis/
            ├── src/views/
            └── src/components/
```

## ⚙️ 核心特性

- **遵循 JHipster 约定**: 生成的代码严格遵循 JHipster 的项目结构和编码规范
- **JPA 专属支持**: 模板专为 Spring Data JPA 设计，充分利用 JpaRepository
- **RESTful 最佳实践**: 符合 JHipster 的 REST 控制器设计模式
- **分页与过滤**: 内置支持 JHipster 风格的分页和过滤查询
- **错误处理**: 集成 JHipster 的标准错误响应格式
- **安全集成**: 支持 JHipster 的安全注解和权限控制
- **Vue.js 前端**: 提供遵循 JHipster 前端约定的 Vue.js 模板

## 🔧 配置示例

以下是一个针对 JHipster 的 `config.xml` 片段示例：

```xml
<config>
    <templates location="backend">
	   <template>
			<id>SPRING_CONTROLLER</id>
			<sourcePath>backend/jhipster/src/main/java/__base_package__/controller/__modelName__Controller.java.vm</sourcePath>
			<targetPath>src/main/java/__basePackage__/controller/__modelName__Controller.java</targetPath>
		</template>
		<template>
			<id>SPRING_REPOSITORY</id>
			<sourcePath>backend/jhipster/src/main/java/__base_package__/repository/__modelName__Repository.java.vm</sourcePath>
			<targetPath>src/main/java/__basePackage__/repository/__modelName__Repository.java</targetPath>
		</template>
    </templates>
    
    <templates location="frontend">
        <profile id="jhipster-vue" label="JHipster Vue 模板" device="desktop">
            <template>
                <id>JHIPSTER_VUE_API</id>
                <sourcePath>frontend/jhipster/vue/src/apis/__modelName__.ts.vm</sourcePath>
                <targetPath>src/main/webapp/app/services/__modelName__Service.ts</targetPath>
            </template>
            <template>
                <id>JHIPSTER_VUE_COMPONENT</id>
                <sourcePath>frontend/jhipster/vue/src/components/__modelName__.vue.vm</sourcePath>
                <targetPath>src/main/webapp/app/entities/__modelName__/__modelName__.vue</targetPath>
            </template>
        </profile>
    </templates>
</config>
```

## 📖 更多信息

关于 **Yimi Crud Generator** 插件的完整功能、详细使用教程、环境变量说明及最新动态，请访问主项目仓库：

**[https://github.com/zhizunyoulan/CRUD-Generator](https://github.com/zhizunyoulan/CRUD-Generator)**

## 🤝 贡献

欢迎提交 Issue 或 Pull Request 来改进这些模板，使其更好地适应不同 JHipster 版本或特定使用场景。