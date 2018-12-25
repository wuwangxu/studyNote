# Gradle
构建工具：
* Ant 编译、测试、打包
* Maven 依赖管理、发布
* Gradle Groovy

## Gradle 是什么
Gradle是一个基于Apache Ant和Apache Maven概念的项目自动化构建开源工具。它使用一种基于Groovy的特定领域语言(DSL)来声明项目设置，抛弃了基于XML的各种繁琐配置。
面向Java应用为主。当前其支持的语言限于Java、Groovy、Kotlin和Scala，计划未来将支持更多的语言。

## 安装
1. 安装JDK
2. 从[Gradle官网](https://gradle.org/)下载压缩包
3. 配置环境变量，`GRADLE_HOME`
4. 添加到path，`%GRADLE_HOME%\bin`
5. 验证是否安装成功，`gradle -v`

## Groovy基础知识
与java的不同
- 不需要写Getter Setter方法
- 可以不写 ;
- 可以不写 return，默认返回最后一条语句
```Groovy
public class ProjectVersion{
    private int major;
    private int minor;

    public ProjectVersion(int major, int minor){
        this.major = major
        this.minor = minor;
    }

    public int getMajor() {
        major
    }

    public void setMajor(int major) {
        this.major = major
    }
}
```

高效特性
```Groovy
// groovy 高效特性
// 1 可选的类型定义
def version = 1

// 2 assert
//assert version == 2

// 3 括号是可选的
println(version)
println version

// 4 字符串
def s1 = 'imooc'
def s2 = "gradle version is ${version}"
def s3 = '''my
name is
wangxu'''
println(s1)
println(s2)
println(s3)

// 5 集合api
def buildTools = ['ant','maven']
buildTools << 'gradle'
assert buildTools.getClass() == ArrayList
assert buildTools.size() == 3

// map ===> hashMap
def buildYears = ['ant':2000,'maven':2004]
buildYears.gradle = 2009
println(buildYears)
println(buildYears.getClass())

// 6 闭包
def c1 = {  // 带参的闭包
    v ->
        println(v)
}

def c2 = {  // 不带参的闭包
    println("hello")
}

def method1(Closure closure){
    closure('param')
}

def method2(Closure closure){
    closure()
}

method1(c1)
method2(c2)
```

## Gradle 基本实例
```Groovy
// 构建脚本中默认都是有个project实例的
apply plugin:'java'

version = '0.1'

repositories {  // 闭包无参
    mavenCentral()
}

dependencies {  // 闭包带参
    compile 'commons-codec:commons-codec:1.6'
}
```

## 自定义任务
```Gradle
def createDir = {   // 带参闭包
    path ->
        File dir = new File(path)
        if (!dir.exists()){
            dir.mkdirs()
        }
}

task makeJavaDir(){
    def paths = ['src/main/java','src/main/resources','src/test/java','src/test/resources']
    doFirst{
        paths.forEach(createDir)
    }
}

task makeWebDir(){
    dependsOn 'makeJavaDir'
    def paths = ['src/main/webapp','src/test/webapp']
    doLast{
        paths.forEach(createDir)
    }
}
```

## 依赖管理
### 常用仓库
mavenLocal/mavenCentral/jcenter/自定义maven仓库/文件仓库（尽少使用）
### 依赖阶段配置
compile -> runtime
testCompile -> testRuntime
```Gradle
// 仅作为示例
dependencies {
    testRuntime group: 'ch.qos.logback', name: 'logback-classic', version: '1.3.0-alpha4'
    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```
### maven仓库配置
```Gradle
repositories {
    maven{  // 私服
        url 'https://maven.aliyun.com/repository/jcenter'
    }
    mavenLocal()    // 本地maven仓库 /user/.m2/repository/
    mavenCentral()
}
```
## 解决版本冲突
### 阻止
```Gradle
configurations.all {
    resolutionStrategy {
        failOnVersionConflict()
    }
}
```
### 解决冲突
* 排除传递性依赖
```Gradle
compile (group: 'org.hibernate', name: 'hibernate-core', version: '5.4.0.Final'){
    exclude group:"org.jboss.logging",module:"jboss-logging"
}
```
* 强制指定一个版本
```Gradle
configurations.all {
    resolutionStrategy {
        failOnVersionConflict()
        force 'org.slf4j:slf4j-api:1.7.24'
    }
}
```
## 多项目构建
### 依赖子项目
```Gradle
compile project(":model")
```
### allprojects
内容与buildprojects一样
```Gradle
allprojects{
    apply plugin: 'java'
    sourceCompatibility = 1.8
}
```

### subprojects
```Gradle
subprojects{
    repositories {
        mavenCentral()
    }

    dependencies {
        testCompile group: 'ch.qos.logback', name: 'logback-classic', version: '1.3.0-alpha4'
        testCompile group: 'junit', name: 'junit', version: '4.12'
    }
}
```