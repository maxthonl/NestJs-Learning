# NestJs Training Outline
> NestJs官方文档[https://docs.nestjs.com/controllers]
## 几个JS概念温习 （作用域 / 变量提升 / 执行上下文 / 闭包）
### 作用域
- ES6之前是没有块级作用域的 - let
``` JS
var myName = 'maxthon';
function showName () {
    console.log(myName); //？
    if (0) {
        // 如何构建块级作用域 ？
        var myName = 'zhangsan';
    }
    console.log(myName); //？
}
showName();
```

### 变量提升
- var 的创建和初始化被提升，赋值不会
- let 的创建被提升，初始化和赋值不会
- function的创建初始化和赋值均被提升
``` JS
showName() // ？
var showName = function() {
    console.log(2);
}
function showName() {
    console.log(1);
}
showName() // ？
```

### 执行上下文
``` JS
let userInfo = {
    name: 'zhangsan',
    age: 35,
    //
    updateInfo: function() {
        setTimeout( function (){
            this.name = 'lisi';
            this.age = 36;
        });
    }
}
userInfo.updateInfo(); // userInfo是否变更了？
```

### 闭包
- 封闭的栈空间，持有外部信息，导致外部引用无法回收。
```JS
function makeFunc() {
    var name = "Mozilla";
    function displayName() {
        console.log(name);
    }
    return displayName;
}

var myFunc = makeFunc();
myFunc();
```

### 经典错误案例
``` JS
// 试着描述下到底发生了什么？
for (var i = 1; i <= 5; i++) {
  setTimeout( function timer() {
      console.log(i);
  }, 1000 );
}
```
---
## NestJS环境搭建
- 方式一
```bash
# 全局安装 NestJs CLI
$ npm i -g @nestjs/cli
# 初始化一个 NestJs 项目
$ nest new project-name

# UnitTest
$ npm i --save-dev @nestjs/testing
```
- 方式二
```bash
$ git clone https://github.com/nestjs/typescript-starter.git project
$ cd project
$ npm install
$ npm run start
```
- 初始化的项目结构介绍
```bash
src
 |-- app.controller.ts
 |-- app.module.ts
 |--main.ts #bootstrap
```
---
## NestJS 相关知识点
### Platform
- platform-express  
- platform-fastify
### Decorators
> NestJs封装了大量的decorator， 同时如果有需要我们也可以实现自己的decorator  

Decorators          |
--|
@Module()           |
@Controller()       |
@Post()             |
@Get()              |
@Query()            |
@Param()            |
@Injectable()       |
@UsePipes()         |
...|

### OpenAPI (Swagger)
- 如何使用Swagger插件
