# maven 学习笔记
## 文件结构

    src
        - main
            - java
                - package
        - test
            - java
                - package
        resource

## mvn 命令
    -v      查看maven版本
    compile 编译
    test    测试
    package 打包
    clean   删除target
    install 安装jar包到本地

### mvn命令创建项目
    1. archetype:generate   按照提示进行选择
    2. archetype:generate 
                -DgroupId=组织名，公司网址的反写+项目名
                -DartifactId=项目名-模块名
                -Dversion=版本号
                -Dpackage=代码所在的包名
## 坐标
构件

## 仓库
本地仓库和远程仓库
## 镜像仓库

## maven 生命周期
1.clean   清理项目
> * pre-clean 执行清理前的工作
> * **clean** 清理上一次构建生成的所有文件
> * post-clean 执行清理后的文件

2.default 构建项目

        compile 编译
        test    测试
        package 打包
        install 安装jar包到本地
3.site    生成项目站点
> * pre-site
> * **site**    生成项目站点的站点文档
> * post-site   生成项目站点后要完成的工作
> * site-deploy 发布生成的站点到服务器上
## maven 依赖
### 依赖传递
A -> B -> C 
exclusion  
排除依赖的依赖
### 依赖冲突（版本）
* 短路优先  依赖传递链的长度
* 长度一样 先声明先用
### 聚合（**module**）
当有多层依赖时，需要依次install,可以使用module标签包含多级依赖，一次install即可自动install所有依赖
### 继承 (**parent**)
当多个模块适用相同的依赖时，可将共同的依赖集成为父maven项目，各子模块使用parent


