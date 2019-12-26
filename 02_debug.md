# NestJs Debug 的三种方式
> 学习的过程中，你可以一知半解，但是得做到会调试，会调试了，啥都不怕。下面我们来了解下用TypeScript写的Nest项目调试的三种方式。

## 1. 浏览器端调试
> 这种调试方法最简单。当我们用nest new命令初始化好一个新项目的时候在 package.json 中已经为我们创建好了npm 脚本命令 `"start:debug": "nest start --debug --watch",` 这里的debug参数其实就是node的inspect模式。当我们用npm run start:debug启动nest app的时候，会在Chrome的开发者工具的左上角中高亮Node图标，这个时候你点击进去，选择source面板展开，就可以尽情调试了。
>
> 这种调试方式只能调试server启动后的逻辑，server启动时的逻辑无法调试。如果专注于项目的业务逻辑而不是nest架构逻辑，那么这就不是一个缺点。
>
> ![image](https://github.com/maxthonl/NestJs-Training/blob/master/images/02_debug_01.png?raw=true)

## 2. tsc 调试
> 比较中规中矩的一种调试方式，在项目的根目录下打开vs code，侧边栏中找到debug并点击进去，一开始如果没有任何调试配置项，vscode会提示你创建一个调试项，跟着向导选择node.js，接着就会初始化一个launch.json文件，该文件就在项目文件夹下的.vscode文件夹下。
>
> 如果你发现 program的指向有问题，说明你在package.json中没有main属性或者main的指向是错误的，不过没关系，只需要在launch.json中修改下即可。
>
> 配置下preLaunchTask，这里nest项目默认使用的tsc配置文件是tsconfig.build.json。具体的task名字可以通过vscode 查询: F1(打开Command Palette) => 输入 tasks => 选择 Tasks: Config Task，这个时候你应该能够看到所有有效的tsc和npm task，因为调试前需要build所以我们选择了tsc: build，当然，你也可以将这个task包成一个npm task然后放进来。
>
> outFiles就是tsconfig中的outDir加上"\*\*/\*.js",这里的\*\*/\*是指递归遍历所有子文件夹的文件。
>
> 如果想使得debug配置文件跨平台下同样生效，在launch.json中添加windows节点，把program和outFiles重新按照windows的方式写一遍即可。
>
>launch.json文件示例如下：
> ```json
> {
>     "version": "0.2.0",
>     "configurations": [
>         {
>             "type": "node",
>             "request": "launch",
>             "name": "TSC_Debug", 
>             "skipFiles": [
>                 "<node_internals>/**"
>             ],
>             "program": "${workspaceFolder}/src/main.ts",
>             "preLaunchTask": "tsc: build - > tsconfig.build.json",
>             "outFiles": [
>                 "${workspaceFolder}/dist/**/*.js"
>             ],
>             "windows": {
>                 "program": "${workspaceFolder}> \\src\\main.ts",
>                 "outFiles": [
>                     "${workspaceFolder}\\dist\\**\\*.js"
>                 ],
>             }
>         }
>     ]
> }
> ```

## 3. ts-node 调试
> 一种很优秀的调试方法，利用ts-node的特性直接调试typescript代码，不需要tsc build命令。首先安装下ts-node:  `npm install ts-node --save-dev`，接着到launch中添加一段配置，然后我们就可以愉快的玩耍了。更多了解请参考github项目[ts-node](https://github.com/TypeStrong/ts-node)  
> 
> 同上，如果想在windows下同样愉快的话，修改下args参数或者添加windows节点把args包裹起来就好了。
>
> launch.json示例如下：
```json
{
    "type": "node",
    "request": "launch",
    "name": "TSNode_Debug",
    "runtimeArgs": [
        "-r",
        "ts-node/register"
    ],
    "args": [
        "${workspaceFolder}/src/main.ts"
    ],
    "windows": {
        "args": [
            "${workspaceFolder}\\src\\main.ts"
        ],
    }
}
```
> 牢记三点：
> 1. 如果需要调试typescript必须将tsconfig中的sourceMap设置为true，否则试想下工具是怎么通过js找到我们的ts文件呢？
> 2. 项目中所有的引用位置都需要设置成相对引用路径，而不是绝对引用路径，试想下如果我们build后所有生成的文件的引用的绝对路径少了一层怎么办？
> 3. 如果是第一种方式，我们需要刷新下浏览器，如果是第二种方式我们需要重新加载debug。

## REST Client
> 推荐个vscode插件“REST Client”。顾名思义，这个插件主要用来测试restful api的，支持curl。用起来也很简单，当你熟练使用后，我想你会忘掉Postman的模样。具体用法如下：
>
> 1. 在项目内新建一个 \<your_name\>.http 文件
> 2. 写入以下格式的API接口：
> ```bash
> ###
> # @name registerNewUser
> POST http://localhost:3000/api/users HTTP/1.1
> Content-Type: application/json
> 
> {"name": "leon", "sex": "unclear"}
>
> ###
> @usid = {{registerNewUser.response.body.$.id}}
> GET http://localhost:3000/api/users/{{usid}}
> ```
> `###`是两个请求的分界线，每个请求以方法类型开头，HTTP版本结尾，接着每一行都是header，空一行后便是body。可以定义一个请求的名字，如上面的`# @name registerNewUser`，然后我们就可以定义一个参数来接收该名字下的某个返回值，如上面的`@usid = {{registerNewUser.response.body.$.id}}`, 这样我们就可以愉快的使用这个参数了，当然这个参数也可以预定义成一个固定值的。
> 更多使用技巧请参考github项目[vscode-restclient](https://github.com/Huachao/vscode-restclient)。
>