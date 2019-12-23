# NestJS CLI 介绍
> 我们可以借助CLI的力量，帮我们完成很多基于NestJS的项目初始化工作。首先需要安装@nestjs/cli
```bash
$ npm i @nestjs/cli -g
```
## $nest info|i
> 该命令用于打印系统信息及Nest版本信息，例如：  

## update|u [options]
> 该命令用于更新Nest版本  

参数|用途
--|--
-f, --force       | 强制更新覆盖本地nest版本
-t, --tag \<tag>  | 指定更新到某一个指定的tag

## $ nest new|n [options] [name]
> 该命令用于创建一个Nest项目

参数|用途
--|--
-d, --dry-run                            | 顾名思义，光打雷不下雨，用于在你执行真正的命令前查看命令的影响范围
-g, --skip-git                           | 忽略git repository的初始化
-s, --skip-install                       | 忽略依赖包的安装，即不执行npm或yarn的install命令
-p, --package-manager [package-manager]  | 指定包管理器 (npm | yarn)
-l, --language [language]                | 指定开发语言 (TS | JS)
-c, --collection [collectionName]        | Collection 用于指定new或generate命令的模板加载位置, 默认是@nestjs/schematics，nest-cli允许我们自行定制模板, 可参考[Build a NestJS Module for Knex.js](https://dev.to/nestjs/build-a-nestjs-module-for-knex-js-or-other-resource-based-libraries-in-5-minutes-12an)

## $ nest build [options] [app]
参数|用途
--|--
-c, --config [path]   | 指定nest-cli的配置文件，默认即项目根目录下的nest-cli.json,主要包括sourceRoot, entryFile, collection, tsConfigPath 等，具体可参考 nest-cli的源码[lib-->configuration-->defaults.js](https://github.com/nestjs/nest-cli/blob/master/lib/configuration/defaults.ts)
-w, --watch           | watch参数，即热重载
--webpack             | 指定使用webpack编译
--webpackPath [path]  | webpack配置文件路径
--tsc                 | 指定使用tsc编译
-p, --path [path]     | tsc配置文件路径与nest-cli.json中的tsConfigPath效果一致

## $ nest start [options] [app]
> 比build命令多了两个参数

参数|用途
--|--
-d, --debug [hostport]   | debug模式运行(node inspect mode)，允许在浏览器中使用nodejs调试 ![image][link1]
-e, --exec [binary]      | 二进制执行文件，默认是node

## $ nest generate|g [options] <schematic> [name] [path]
> 该命令用于根据模板初始化项目文件,schematic列表如下：

参数名 | 别名    
--|--
application   | application
angular-app   | ng-app     
class         | cl         
configuration | config     
controller    | co         
decorator     | d          
filter        | f          
gateway       | ga         
guard         | gu         
interceptor   | in         
interface     | interface  
middleware    | mi         
module        | mo         
pipe          | pi         
provider      | pr         
resolver      | r          
service       | s          
library       | lib        
sub-app       | app        

> 具体参数如下：

参数|用途
--|--
--dry-run                          | 光打雷不下雨，用于在你执行真正的命令前查看命令的影响范围
-p, --project [project]            | 指定为哪个项目生成该文件
--flat                             | 打平生成文件结构，例如生成controller [name]的时候会自动归类到[name]文件夹下，打平后，会生成到nest-cli的sourceRoot路径下
--no-spec                          | 不生成对应的测试文件
-c, --collection [collectionName]  | Collection 用于指定new或generate命令的模板加载位置，参考nest new命令

## $ nest add <library> [args...] 
> 具体作用不详，还请大佬能指点一二。官方解释：Imports a library that has been packaged as a nest library, running its install schematic.貌似应该是辅助nest genarate 命令做自定义模板的

---
[link1]:data:image/png;base64,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
