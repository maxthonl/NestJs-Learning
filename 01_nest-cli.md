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
-d, --debug [hostport]   | debug模式运行(node inspect mode)，允许在浏览器中使用nodejs调试 ![image](https://github.com/maxthonl/NestJs-Learning/blob/master/images/01_nest-cli_01.png?raw=true)
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
