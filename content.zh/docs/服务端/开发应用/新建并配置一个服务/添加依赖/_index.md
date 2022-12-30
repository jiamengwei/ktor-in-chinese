---
BookCollapseSection: true
---

# 添加Ktor依赖
本章节，我们将向你演示如何为已经存在的Gradle或Maven项目添加Ktor必须的依赖
## 仓库配置
添加Ktor依赖之前，你需要配置项目的依赖仓库： 
#### 生产仓库
  Ktor的生产发布版在maven中心仓库可用，你可以像下面这样引用仓库
1. Gradle(Kotlin)
```Kotlin
repositories {
    mavenCentral()
}
```
2. Gradle(Groovy)
```Groovy
repositories {
    mavenCentral()
}
```
3. Maven
```
Maven中央仓库已经包含相关依赖，无需额外添加
```

#### 预览版仓库（EAP）
你需要引用这个[仓库](https://maven.pkg.jetbrains.space/public/p/ktor/eap/io/ktor/)来访问[预览版](https://ktor.io/eap/)本的Ktor：
1. Gradle(Kotlin)
```Kotlin
repositories {
    maven {
        url = uri("https://maven.pkg.jetbrains.space/public/p/ktor/eap")
    }
}
```
2. Gradle(Groovy)
```Groovy
repositories {
    maven {
        url "https://maven.pkg.jetbrains.space/public/p/ktor/eap"
    }
}
```
3. Maven
```XML
<repositories>
    <repository>
        <id>ktor-eap</id>
        <url>https://maven.pkg.jetbrains.space/public/p/ktor/eap</url>
    </repository>
</repositories>
```
注意：Ktor预览版可能需要[Kotlin开发版仓库](https://maven.pkg.jetbrains.space/kotlin/p/kotlin/dev)：
1. Gradle(Kotlin)
```kotlin
repositories {
    maven {
        url = uri("https://maven.pkg.jetbrains.space/kotlin/p/kotlin/dev")
    }
}
```
2. Gradle(Groovy)
```Groovy
repositories {
    maven {
        url "https://maven.pkg.jetbrains.space/kotlin/p/kotlin/dev"
    }
}
```
3. Maven
```XML
<repositories>
    <repository>
        <id>ktor-eap</id>
        <url>https://maven.pkg.jetbrains.space/kotlin/p/kotlin/dev</url>
    </repository>
</repositories>
```
## 添加依赖
### 核心依赖
每一个Ktor应用都必须包含以下依赖：

•	ktor-server-core: 包含ktor的核心功能

•	服务引擎的依赖（示例，ktor-server-netty）

针对不同的平台ktor提供了不同的带有平台后缀（-jvm）的包，例如：ktor-server-core-jvm或ktor-server-netty-jvm. 需要注意的是Gradle会解析平台的依赖包，maven需要手动维护，依赖如下所示：
1. Gradle(Kotlin)
```kotlin
dependencies {
    implementation("io.ktor:ktor-server-core:2.2.1")
    implementation("io.ktor:ktor-server-netty:2.2.1")
}
```
2. Gradle(Groovy)
```Groovy
dependencies {
    implementation "io.ktor:ktor-server-core:2.2.1"
    implementation "io.ktor:ktor-server-netty:2.2.1"
}
```
3. Maven
```XML
<dependencies>
    <dependency>
        <groupId>io.ktor</groupId>
        <artifactId>ktor-server-core-jvm</artifactId>
        <version>2.2.1</version>
    </dependency>
    <dependency>
        <groupId>io.ktor</groupId>
        <artifactId>ktor-server-netty-jvm</artifactId>
        <version>2.2.1</version>
    </dependency>
</dependencies>
```

### 日志依赖
Ktor使用SLF4J作为记录应用日志的接口，SLF4J有多个实现可供选择，例如：Logback，Log4j， 学习如何添加日志依赖请看[添加日志依赖](https://ktor.io/docs/logging.html#add_dependencies)。
### 插件依赖
[插件](https://ktor.io/docs/plugins.html)指ktor的扩展包，详情可以在相关章节了解。

## 创建应用程序入口
通过Gradle或maven [运行一个Ktor服务](https://ktor.io/docs/running.html)取决于创建服务的方式。
你可以使用以下方法中的一个来指定应用的启动类：

•	如果你使用嵌入式服务（[embeddedServer](https://ktor.io/docs/create-server.html#embedded-server)）， 指定主启动类像下面这样:
1. Gradle(Kotlin)
```kotlin
application {
    mainClass.set("com.example.ApplicationKt")
}
```
2. Gradle(Groovy)
```Groovy
application {
    mainClass = "com.example.ApplicationKt"
}
```
3. Maven
```XML
<properties>
    <main.class>com.example.ApplicationKt</main.class>
</properties>
```

•	如果你使用引擎类，你需要配置它作为主启动类，示例：指定Netty作为引擎主启动类
1. Gradle(Kotlin)
```kotlin
application {
    mainClass.set("io.ktor.server.netty.EngineMain")
}
```
2. Gradle(Groovy)
```Groovy
application {
    mainClass = "io.ktor.server.netty.EngineMain"
}
```
3. Maven
```XML
<properties>
    <main.class>io.ktor.server.netty.EngineMain</main.class>
</properties>
```

> ⚠️ 如果你要将你的应用打包为一个Fat JAR，你需要考虑当配置对应的插件时服务创建的方式，你可以在下面的文章中了解更多：
> * [使用Ktor Gradle 插件创建fat JARs](https://ktor.io/docs/fatjar.html)
> * [使用Maven Assembly 插件创建fat JARs](https://ktor.io/docs/maven-assembly-plugin.html)