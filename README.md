# JHipster Templates for Yimi Rrud Generator

## Overview

This is a set of JHipster companion template files designed specifically for the **Yimi Rrud Generator** code generation plugin.

## Core Purpose

These templates allow you to generate code in **Yimi Rrud Generator** that complies with JHipster project specifications, conventions, and best practices, including standard REST controllers, Service layer, Repository layer, and related configurations.

## 🚀 Quick Start

### Prerequisites
- Installed **Yimi Rrud Generator** IntelliJ IDEA plugin
- An existing Spring Boot project created with JHipster

### Key Configuration
**Important**: Before using this template set, **you must set the framework type to `JPA` in the plugin settings**, instead of the default MyBatis.

### Usage
1. Clone or download this repository to your local machine
2. Copy the `templates` folder from this repository to the **root directory** of your project
3. Configure **Yimi Rrud Generator** plugin to use these templates
4. Select your JPA Entity class to start generating JHipster-compliant code

## 📁 Template Contents

This repository contains the following templates for JHipster:

```
templates/
├── backend/
│   └── jhipster/                 # JHipster backend templates
│       ├── src/main/java/__base_package__/
│       │   ├── controller/      # REST controllers (following JHipster RESTful conventions)
│       │   ├── service/         # Service layer (using constructor injection)
│       │   └── repository/      # Spring Data JPA repository interfaces
│       └── src/main/resources/
│           └── config/          # Configuration file templates
└── frontend/
    └── jhipster/
        └── vue/                 # Vue.js frontend templates
            ├── src/apis/
            ├── src/views/
            └── src/components/
```

## ⚙️ Core Features

- **JHipster Compliance**: Generated code strictly follows JHipster project structure and coding standards
- **JPA Exclusive Support**: Templates designed specifically for Spring Data JPA, utilizing JpaRepository fully
- **RESTful Best Practices**: Adheres to JHipster's REST controller design patterns
- **Pagination & Filtering**: Built-in support for JHipster-style pagination and filtering queries
- **Error Handling**: Integrated with JHipster's standard error response format
- **Security Integration**: Supports JHipster security annotations and permission controls
- **Vue.js Frontend**: Provides Vue.js templates following JHipster frontend conventions

## 🔧 Configuration Example

Here is an example `config.xml` snippet for JHipster:

```xml
<config>
    <templates location="backend">
        <profile id="jhipster" label="JHipster JPA Templates">
            <template>
                <id>SPRING_CONTROLLER</id>
                <sourcePath>backend/jhipster/src/main/java/__base_package__/controller/__modelName__Controller.java.vm</sourcePath>
                <targetPath>src/main/java/__package__/controller/__modelName__Controller.java</targetPath>
            </template>
            <template>
                <id>SPRING_REPOSITORY</id>
                <sourcePath>backend/jhipster/src/main/java/__base_package__/repository/__modelName__Repository.java.vm</sourcePath>
                <targetPath>src/main/java/__package__/repository/__modelName__Repository.java</targetPath>
            </template>
        </profile>
    </templates>
    
    <templates location="frontend">
        <profile id="jhipster-vue" label="JHipster Vue Templates" device="desktop">
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

## 📖 More Information

For complete features, detailed tutorials, environment variable documentation, and latest updates about the **Yimi Rrud Generator** plugin, please visit the main project repository:

**[https://github.com/zhizunyoulan/CRUD-Generator](https://github.com/zhizunyoulan/CRUD-Generator)**

## 🤝 Contributing

Issues and Pull Requests are welcome to improve these templates for better compatibility with different JHipster versions or specific use cases.

