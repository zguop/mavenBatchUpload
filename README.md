# gradle task uploadBatch maven私服一键deploy工具   

### 项目介绍
工具基于gradle task 编写，只要简单的引入.gradle文件，配置打包模块，就可以轻松实现一键批量打包功能，
该项目适用于：公司业务基础组件lib存放于公司maven仓库。


### 使用方式

### 涉及到的文件
* nexus_maven.gradle :
* nexus_maven_batch.gradle 
* nexus_maven_config.properties 
将三个文件保存在一个目录下在gradle.properties下配置包名，如：
```groovy
MAVEN_UPLOAD_DIR=maven_upload
```

### 配置properties
基础配置：nexus_maven_config.properties 
```groovy
RELEASE_URL=远程仓库release地址
SNAPSHOT_URL=远程仓库snapshot地址
GROUP_ID=统一包的GroupId
NAME=远程账号
PASSWORD=远程密码
EXCEPTION_MODULE=这个可以是用记录打包中时记录当前模块时用的，需要配置key，但不用配置value，脚本代码中有使用到这个字段
```

模块配置按照moduleName=versionName方式进行配置,如：
```groovy
a_lib=1.0.0 
```
### 引入gradle
nexus_maven_config.properties配置的模块都需要在相应的模块下引入maven插件
```groovy
apply from: "../$MAVEN_UPLOAD_DIR/nexus_maven.gradle"
```
最后在项目根build.gradle引入批量打包的脚本
```groovy
apply from: "$MAVEN_UPLOAD_DIR/nexus_maven_batch.gradle"
```
 以上就配置完成了。
 
### 使用方式 （重要）
*  ./gradlew uploadBatch

*  可选参数：-PM=指定你要打包的模块, -PD,  -PL

*  参数解释: -PM：指定打包模块名称(不指定PM参数将配置文件中的所有模块全量打包) -PD：snapshot -PL：增加系统日志输出

*  打包流程：例：执行./gradlew uploadBatch -PM=a_lib，脚本会分析这个模块的依赖关系排列出一个打包列表，列表内所有模块将统一执行uploadArchives

*  版本管理：最好使用三位数版本号(初始化版本为：1.0.0)，脚本循环执行上传，因此再每执行成功一个模块时，该配置文件中的版本号默认尾号+1升级

*  异常处理：脚本过程中的异常处理，当发生异常中断时，不要慌！！！脚本会记录发生错误的位置，当你修改完错误后，重新执行该命令(命令执行必须和上一次保持一致)，可以从错误节点开始继续打包


总结
-
xiexie ni de guāng gù ！ 喜欢的朋友轻轻右上角赏个star，您的鼓励会给我持续更新的动力。








