# NPM - Node Package Manager
> 顾名思义，就是Node的包管理器，随Node的安装包一起提供给开发者。在NPM没有出来之前，都是自己去各大网站搜索想要的js库，然后自行下载并引用。当然，比如google也提供了一些js库的网络链接，使得开发者无须下载。但是随着NPM的普及，这些散落在各地的js库最终汇聚到了一个registry内，同时引入package.json，使得每个发布的包都有自己对应的依赖包版本管理，很好的解决了版本冲突问题。下面我们来介绍下npm的常用命令：
## `npm init`
> 在开发一个全新的项目的时候我们需要对一些基本的项目信息做初始化动作，即`npm init`，terminal进入项目文件夹内，执行`npm init`命令，按照控制台提示一步步完成操作即可，完成后会在项目文件夹内生成项目的package.json文件，简单的来看下这个文件:

```js
{
  "name": "test-npm",  // 默认项目文件夹名字，你也可以指定别的名字
  "version": "1.0.0", // 项目版本号
  "description": "this is a training demo for `npm init`", // 项目描述
  "main": "index.js", // 入口文件，调试时会用到
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }, // npm 脚本，除了start外都需要在这个位置定义，比如 build、test等
  "author": "", // 响当当的名号
  "license": "ISC" // 授权许可类型，法务最在意的东西，身为程序员，可不要当法盲哦，具体的可以网上恶补
}
```
> 这里少了两个很关键的配置项，在`npm install`中会提到，接下来我们来看怎么引用第三方类库。

## `npm install`
> 一般情况下如我们想要使用某个类库，这时候我们需要做以下几步：
1. 到 https://www.npmjs.com/package 搜索自己想要的类库，并进入搜索项；
2. 这里有两个选项卡我们需要注意：  
Readme : 一般会有类库的相关介绍及使用方法；
Versions : 版本变更记录，当我们想安装该类库的某一特定版本的时候；
3. 在我们的项目文件夹下执行`npm install XXX`，静待佳音即可。
> 很多时候，小白们傻傻分不清哪些包需要安装到全局环境，哪些包需要安装到开发依赖项。给大家一个简单的判断标准，如果是工具类库比如nrm、cnpm，就使用参数 `--global`，如果是项目内使用，但是打包后不需要的，比如jest、tslint，就使用参数 `--save-dev`。如果想安装特定的版本就在类库的后面跟上版本号`@X.X.X`，以下是参考命令：
```bash
$ npm install uuid@7.0.3
$ npm install nrm --global
$ npm install tslint --save-dev
```
> 安装完第三方类库后，我们再次去查看下项目文件夹，我们会发现多了几样东西：
- node_modules  
这个文件夹下是项目引用到的所有三方类库存放的位置；
- package-lock.json  
这个文件在CICD中非常有用，顾名思义锁定依赖项版本， 提供一个稳定的测试及生产环境；
- package.json  
这个文件会新增两个配置项`devDependencies`、`dependencies`，这里列出来的所有包都会在node_modules中出现。
> 我们也可以先在package.json中添加我们想要引用的三方库，然后运行`npm install`命令，这时就会在执行路径下搜索package.json文件并读取依赖项执行安装操作。
> 相应的，如果想卸载某个依赖包就执行命令`npm uninstall XXX`

## `npm run`
> 说完了项目引用，我们再来讲一讲项目的构建打包，JS的构建打包有很多，比如Gulp、Webpack、lerna等等，命令多了也记不住会，而且如果需要的参数很多，每次都敲一遍很累的，也容易出错。这个时候，我们可以将自己的脚本配置到package.json中，将命令变得简单而且更语义化，例如：
```json
"scripts": {
    "build": "tsc -p tsconfig.build.json",
    "format": "prettier --write \"src/**/*.ts\""
}
```
> 这个时候我们就可以在terminal中使用简单的npm命令了，`npm run build`， 看到build我们就知道，哦～，这个是用来构建的， `npm run format`，看到format我们就知道， 哦～，这个是用来格式化的。`npm start`是一个特殊的script，不需要在scripts中有所体现，除非你想控制它。

## npm publish
> 自己做完了，想普度众生咋办呢？这个时候我们就可以把自己的项目以包的形式发布到npm registry内，这样，别人就可以引用你发布的包了。做事情是要负责的，不然你把公司的代码发上去试试？所以我们需要注册npm的账户`npm adduser`。 在发布前我们需要`npm login`，这时我们接着执行`npm publish`命令即可，如果想要下架也是允许的`npm unpublish XXX[@X.X.X] [--force]`  
- 记住，这里的user是在你当前的registry内的！！

## `npm config [get|set] registry`
> npm 也有自己的配置项，这里只说明下registry，registry指的就是npm包的源，因为GWF或者私有registry的原因，我们需要切换registry的地址。
```bash
$ npm config get registry
https://registry.npmjs.org/ # npm官方registry地址 
$ npm config set registry https://registry.npm.taobao.org/ # 淘宝镜像源
```
> 针对记忆力不好的人，比如我，就喜欢使用工具 nrm（npm registry manager）来解决问题
```bash
$ npm i nrm -g # npm install nrm --global
$ nrm ls # 列出所有配置好的registry，淘宝镜像源也在默认的列表内
$ nrm use XXX # 切换registry，比如 XXX = taobao
$ nrm add maxthonLib http://localhost:8080 # 增加新的registry地址
```
## Verdaccio
> 上节我们提到registry可以配置私有的，Verdaccio无疑是其中的佼佼者，没有使用过docker的开发者正好可以借着这个机会来简单的入门docker。镜像地址：https://hub.docker.com/r/verdaccio/verdaccio， 按照Overview上的操作一顿噼里啪啦就行了，这里就不赘述了。