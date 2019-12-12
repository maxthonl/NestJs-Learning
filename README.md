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
---
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
---
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
---
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
---
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
## NestJS
### 环境搭建
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
 |-- app.controller.ts # controller
 |-- app.module.ts # export module
 |-- main.ts # bootstrap
#|-- app.service.ts
```
- CLI 相关命令
```bash
$ nest -h
$ nest g -h
```
---
### Platform
> NestJs的默认平台是express。
- platform-express  
- platform-fastify
### Module
> @Module 装饰器用来声明一个module。可以用@Global来声明这个module是全局的，也可以使用forRoot（）来动态创建一个模块
```js
@Global()
@Module({
    providers: [DatabaseProvider]
})
export class DatabaseModule {
    static async forRoot(env: string) {
         const provider =  createDatabaseProvider(env); // 根据环境变量连接不同的数据库
         return {
             module: DatabaseModule,
             providers: [provider],
             exports: [provider]
         }
    }
}
```
---
### Controller
> 负责请求的分发处理, 我们可以通过CLI命令来简单创建controller：  
> *nest g controller [name]*
1. Method Decorators - *@Post, @Get...*
2. Route parameters - *@Get(':XXX')*
3. Route wildcards - *@Get('a\*s')*
4. Status Code - *@HttpCode*
5. Headers - *@Header*
6. Request Parameters - *@req, @Body, @Param, @Query, @Headers*
```js
import { Controller, Get, Query, Post, Body, Put, Param, Delete } from '@nestjs/common';
import { CreateCatDto, UpdateCatDto, ListAllEntities } from './dto';

@Controller('cats')
export class CatsController {
  @Post()
  @Header('Cache-Control', 'none')
  create(@Body() createCatDto: CreateCatDto) {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(@Query() query: ListAllEntities) {
    return `This action returns all cats (limit: ${query.limit} items)`;
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return `This action returns a #${id} cat`;
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return `This action removes a #${id} cat`;
  }
}
```
---
### Providers
> NestJs 使用 @Injectable() 装饰需要inject的类，可以是services utils等
1. Scope - *__SINGLETON__, REQUEST, TRANSIENT*
2. Types - *Class-based, Property-based, Optional*
3. Registration - *useValue, useClass, useFactory, useExisting*
```js
// 一个injectable 的 service类
@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```
---
### Middleware
> 我们可以创建两种 Middleware 1. class-based， 2. function-based
```js
// 创建自己的middleware
@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: Function) {
    console.log('Request...');
    next();
  }
}

// 在module的configuratio中使用
@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats');
  }
}
```
---
### Pipe
> 
```bash
$ npm i --save class-validator class-transformer
```

---
### Decorators
> NestJs封装了大量的decorator， 同时如果有需要我们也可以实现自己的decorator  

Decorators |
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
- 如何创建Swagger
