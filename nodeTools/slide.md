title: Node工具化
speaker: 刘放

<slide class="bg-black-blue aligncenter" image="http://edu-image.nosdn.127.net/bbe5be1eda96418aaa409495f8b76a00.png?imageView&quality=100 .dark">

# node工具化对前端开发提效的实践 {.text-landing.text-shadow}

<slide :class="size-30 aligncenter">

## 自我介绍
---

* Name 刘放 {.animated.fadeInUp}
* Web前端开发@有道云课堂 {.animated.fadeInUp.delay-400}
* Github brizer {.animated.fadeInUp.delay-800}
* Wechat brizer1992 {.animated.fadeInUp.delay-1200}

<slide >

:::{.content-left}

### 分享大纲

:::flexblock {.specs}

## Node背景介绍

---

## 脚手架

---

## 多工程管理

---

## Mock

---

## 活动页面生成系统

<slide :class="size-50 aligncenter">

## Node背景介绍
 
<slide class="aligncenter">

## Node到底是什么？

Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.{.lightSpeedIn.animated.slow}

<slide image="http://edu-image.nosdn.127.net/743a89b32a714af58455114b02d73508.jpg?imageView&quality=100 .right">

:::{.content-left}

### Node的组成

:::flexblock {.specs}

## Application

你写的node应用或者说模块

---


## Modules

Node.js 标准库，对外提供的 JavaScript 接口，例如模块 http、buffer、fs、stream 等

---

## C++ Binding

JavaScript 与 C++ 连接的桥梁，对下层模块进行封装，向上层提供基础的 API 接口如zlib、OpenSSL、c-ares、http-parser 等

---

## Addons

JavaScript 与 C++ 连接的桥梁，第三方胶水层代码

:::


<slide image="http://edu-image.nosdn.127.net/743a89b32a714af58455114b02d73508.jpg?imageView&quality=100 .right">

:::{.content-left}

### Node的组成

:::flexblock {.specs}

## V8

Google 开源的高性能 JavaScript 引擎，以 C++ 实现。这也是集成在 Chrome 中的 JS 引擎

---

## libuv

提供异步功能的 C 库。它在运行时负责一个事件循环（Event Loop）、一个线程池、文件系统 I/O、DNS 相关和网络 I/O，以及一些其他重要功能。 每个操作系统对于事件多路复用器有其自身的接口，Linux是epoll，Mac OSX是kqueue，Windows的IOCP API


:::

<slide>

## Node的应用场景

* :跨平台\::{.text-label}  传统的PC Web端，以及PC客户端 nw.js/electron 、移动端 cordova、HTML5、react-native、weex，硬件 ruff.io...
* :后端应用开发\::{.text-label}  网站、Api、RPC服务，比如Express/Koa/Egg/Nest/Hapi/Restify/Socket.io...
* :前端基建\::{.text-label} 三大框架 React/Vue/Angular 辅助开发，以及工程化演进如grunt/gulp/webpack/rollup/parcel/snowpack/rollup...
* :命令行工具\::{.text-label} npm上各种模块如commander/yargs/shelljs/lerna/http-server/node-http-proxy...
{.description}



<slide class="fullscreen">

:::card

![](http://edu-image.nosdn.127.net/74f66c2ae7c54e8dbadba9cbfe566c7b.jpg?imageView&quality=100)

---

## Node的生态优势

数据源引自[modulecounts](http://www.modulecounts.com/){.text-intro}

更多Node使用场景及生态请参考

[awesome-node](https://github.com/sindresorhus/awesome-nodejs){.text-intro}

[awesome-url](https://brizer.github.io/urls/zh/node_modules_zh.html){.text-intro}


:::

<slide >

:::{.content-left}

### 分享大纲

:::flexblock {.specs}

## Node背景介绍

---

## 脚手架

---

## 多工程管理

---

## Mock

---

## 活动页面生成系统

<slide :class="size-50 aligncenter">

## 脚手架

<slide image="http://edu-image.nosdn.127.net/0f9dfc5e2c594eda96bad2a0d1cb3b5c.jpg?imageView&quality=100 .right">

:::{.content-left}

### 一个标准的脚手架思路


* :命令注册\::{.text-label}  commander/yargs...
* :git拉取\::{.text-label}  download-git-repo/@gitbeaker...
* :交互\::{.text-label} inquirer/ora...
* :模板\::{.text-label} Handlebars/EJS/Jade...


:::


<slide :class="aligncenter size-50">

## 从头自建脚手架

----

* 重复造轮子！
* 劳民伤财！
* 稳定性不可保障，维护成本高
* 耦合设计，拓展性不高


<slide  class="bg-white aligncenter" image="http://edu-image.nosdn.127.net/53c9e1ec8c1744d9afd0e8b55a597783.jpg?imageView&quality=100 .dark">

## 合理利用生态优势，拥抱开源

| 库 | 使用场景 | 定制方式 |
| :------------: | :------------: | :------------: |
| yeoman   |   初始化工程   |   定制化generator，插件使用 |
| plop    |    工程内新增或修改文件    |      定制交互prompts及操作actions |


<slide :class="size-50 aligncenter">

## 脚手架1.0

基于plop，工程内定制

<slide :class="size-50 aligncenter">

### 工程内置模板及配置

![img](http://edu-image.nosdn.127.net/6fa5409566a54906857f834086cf49a1.jpg?imageView&quality=100)

<slide :class="size-50 aligncenter">

### 自定义交互及行为

``` javascript
module.exports = {
    prompts: [{//...各种不同类型的交互
            type: "list",
            message: "请选择新增的页面位于哪个一级路由下:",
            name: "scope",
            choices: pageList
        },{
            type: "input",
            name: "chineseName",
            message: "请输入页面中文名称（例如开屏配置）:"
        }],
    actions: data => {//获取交互中的得到的参数
        const { scope, name, chineseName } = data;
        return [{//...各种不同类型的行为
                type: "add",
                path: `src/javascript/admin/module/${scope}/${name}.js`,
                templateFile: "plop-templates/page/module.hbs",
                data: {umiName}
            },{
                type: "modify",
                path: `src/javascript/admin/config/admin.js`,
                templateFile: "plop-templates/page/config.hbs",
                pattern: /var exports = \{/,
                data: {name}
            }];
    }
};
```

<slide :class="size-50 aligncenter">

### 效果展示

![img](http://edu-image.nosdn.127.net/33b04d8c801e4782b9062bef0da37db7.gif?imageView&quality=100)


<slide :class="size-60">

### 遗留问题


---

:::shadowbox

## 模板在工程内维护，那么跨工程的通用模板不好复用

---

## 每次新增都需要改到配置文件，可维护性不怎么好

:::


<slide :class="size-50 aligncenter">

## 脚手架2.0

基于node-plop，分离模板库和脚手架

<slide :class="size-50 aligncenter">

## 整体思路

![](http://edu-image.nosdn.127.net/dbd9249f0f9f4b4ab1cf29ba131f59b2.jpg?imageView&quality=100)


<slide :class="size-50 aligncenter">

## 物料库风格

``` shell
├── blocks - 各个物料本身的模板
|  ├── bower
|  |  ├── bower.hbs
|  |  └── bowerrc.hbs
|  ├── example
|  |  └── demo.hbs
|  ├── mocker
|  |  ├── dev.hbs
|  |  ├── httpmockrc.hbs
|  |  └── package.hbs
|  ├── roll
|  |  ├── dev.hbs
|  |  └── package.hbs
|  └── singleVue
|     └── index.hbs
└── plop-templates - 各个物料自己的命令行交互及具体文件流操作
   ├── bowerGenerator.js
   ├── exampleGenerator.js
   ├── mockerGenerator.js
   ├── rollGenerator.js
   └── singleVueGenerator.js

```

<slide :class="aligncenter">

### 遗留问题（todo）


---

:::shadowbox

## 物料不能做到本地开发，调试不便（打算通过cli强化物料开发流程）

---

## 工程内需要到指定文件路径执行命令（打算通过vscode扩展或直接配上ui解决）

:::


<slide >

:::{.content-left}

### 分享大纲

:::flexblock {.specs}

## Node背景介绍

---

## 脚手架

---

## 多工程管理

---

## Mock

---

## 活动页面生成系统

<slide :class="size-50 aligncenter">

## 多工程管理

<slide class="aligncenter">

## 工程数量 

10+ {.text-data}

## 组件数量

60+ {.text-data}



<slide class="bg-white aligncenter" image="http://edu-image.nosdn.127.net/381e4d34c4c34f1b89499ddecbabe6df.png?imageView&quality=100 .dark">

## WTF

### 每次预发上线都头大？
 

<slide>

## 合理利用生态优势，拥抱开源


| 库 | 特点 | 特点 | 场景 |
| :------------: | :------------: | :------------: | :------------: |
| lerna   |   mono-repo   |   同一git仓库下，管理多个包，包含开发及发布完整流程 | util库及组件库 |
| meta    |    multi-repo    |   同一文件结构下，管理多个不同仓库，支持插件扩展子命令，管理git等 | 工程群 |
| multi-repo-git  |    multi-repo    |   通过全局配置文件，跨文件目录管理不同仓库，定制化git及包依赖，不支持插件 | 工程群 |

<slide :class="size-50 aligncenter">

### 效果展示

![img](http://edu-image.nosdn.127.net/b12dddc6276f46b5a355550e2317ac90.gif?imageView&quality=100)



<slide :class="size-40 aligncenter">

### 核心原理

---

``` js
//透传参数，子进程指定路径执行
child_process.exec(command,{
    cwd:'配置文件读取的工程路径'
})
```


<slide >

:::{.content-left}

### 分享大纲

:::flexblock {.specs}

## Node背景介绍

---

## 脚手架

---

## 多工程管理

---

## Mock

---

## 活动页面生成系统

<slide :class="size-50 aligncenter">

## Mock

<slide :class="aligncenter size-50">

## 哪些场景需要Mock

----

* 前端本地开发时，对依赖的后端接口的Mock
* 服务A开发时，对依赖的服务B的Mock
* 单元测试时，对某些模块或方法的Mock

<slide  class="bg-white aligncenter" image="http://edu-image.nosdn.127.net/4c56650f90ca4dd48eeed55fc226cefd.jpg?imageView&quality=100 .dark">

## 接口Mock选型

| 库 | 特点描述 | 使用场景 |
| :------------: | :------------: | :------------: |
| yapi   |   重型平台，功能齐全   |  统一管理所有工程接口 |
| rap2-delos    |    重型平台，阿里妈妈出品    |      统一管理所有工程接口 |
| nei    |    重型平台，网易自研    |      统一管理所有工程接口 |
| http-mocker    |    笔者自研，轻量级    |   组件内或util等example对应mock    |

<slide  class="aligncenter">

## 轻量级场景

#### 你是直接代码侵入式修改mock？

``` js
const actions = {
    save: isLocal? '/mock/success.json':`/p/train/config/submit.do?courseId=${g.courseId}`,
    detail: (isLocal?'/mock':'') + '/j/train/config/init.json',
}

export default {
    save(data):Promise<boolean>{
        console.warn(data)
        return baseSvs[isLocal?'get':'post'](actions.save, data)
    },
    detail(courseId):Promise<any>{
        return baseSvs.get(actions.detail+'?courseId='+courseId)
    },
}

```

<slide  class="aligncenter">

#### 还是启动一个mock服务，来响应所有接口？

``` js
import Mock from 'mockjs'

export default [
  // mock get all routes form server
  {
    url: '/article/pv',
    type: 'get',
    response: _ => {
      return {
        code: 20000,
        data: {
          pvData: [
            { key: 'PC', pv: 1024 },
            { key: 'mobile', pv: 1024 },
            { key: 'ios', pv: 1024 },
            { key: 'android', pv: 1024 }
          ]
        }
      }
    }
  },
]
```

<slide :class="aligncenter size-50">


* 侵入代码影响性能和可读性！

* 额外启动一个服务很繁琐！

* 无法动态切换某一个接口本地和远程模式！

* 每个规则都需要手写js，麻烦！



<slide  class="aligncenter  size-60">

## 解决方案 Http-mocker

#### 如何不启动服务、不侵入也可以进行mock? 

直接将express或webpack-dev-server的app对象传入

``` js
const { mocker } = require('http-mockjs')
const webpackConfig = {
  devServer: {
    before:app=>{
      mocker(app)
    }
  }
}
```

<slide  class="aligncenter size-60">

#### 如何识别路由，并动态切换本地或远程? 

利用`app.all('/*')`，以`path-to-regexp`风格拦截路由

``` js
  app.all("/*",async (req, res, next) => {
    const proxyMatch = getMatechedRoute(proxyLists, proxyURL);
    // 匹配到对应路由
    if (proxyMatch && proxyMatch.ignore !== true) {
      // 延时返回功能
      if(proxyMatch.delay && typeof proxyMatch.delay === 'number'){
        await sleep(proxyMatch.delay)
      }
      // 校验入参格式
      if(proxyMatch.validate && !isEmptyObject(proxyMatch.validate)){
        //...
      }
      let responseBody;
      // 同时支持js模式，沙盒运行
      if(/js$/ig.test(curPath) ){
        responseBody = vm.run(jsContent)(req);
      }else{
        // json模式直接读取返回值
        responseBody = fs.readFileSync(curPath, "utf-8");
      }
      // 支持mockjs来灵活化数据
      const result = mock.mock(responseBody);
      // 自定义响应头 
      res.set(responseHeaders);
      res.send(result);
      res.end();
    }
  });
```

<slide class="bg-black slide-top" image="http://edu-image.nosdn.127.net/f4b0ed19008d459cb48420c3ef848319.jpg?imageView&quality=100">

:::{.content-center}

#### 如何优化配置的便利性? 

抽出独立的ui包，可视化编辑



<slide >

:::{.content-left}

### 分享大纲

:::flexblock {.specs}

## Node背景介绍

---

## 脚手架

---

## 多工程管理

---

## Mock

---

## 活动页面生成系统

<slide :class="size-50 aligncenter">

## 活动页面生成系统


<slide class="fullscreen">

:::card

![](http://edu-image.nosdn.127.net/85613f39d38844ff889a45bb97811adb.jpg?imageView&quality=100)

---


没有开发介入的情况下，运营自己完成页面的整体搭建 {.text-intro}

应用场景\:

* 促销活动页，[云课堂618大促为例](https://study.163.com/topics/2020xxj#/){.text-intro}

:::


<slide class="bg-black aligncenter" image="http://edu-image.nosdn.127.net/6972ae45a8dd470ca8bd40ce9d578b59.jpg?imageView&quality=100 .dark">

## 完成页面数量 

9700+ {.text-data}

## 独立模块数量

120+ {.text-data}


<slide :class="size-50 aligncenter">

### 整体架构

![](http://edu-image.nosdn.127.net/0c3530ef6c824a5aa0da71ce03d43030.jpg?imageView&quality=100)


<slide :class="aligncenter">

### 模块元数据

``` json
{
    "chineseName": "移动端课程卡片带切换(抢课,拼团,提醒,收藏)",
    "content": "<ux-cms-course-list-with-tab tabList={list}></ux-cms-course-list-with-tab>",
    "dataTemplate": "c2_courselist_tab",
    "dependence": "pool/component-cms/src/course-list-with-tab/wap/ui",
    "description": "移动端课程卡片带切换(抢课,拼团,提醒,收藏)",
    "name": "m_course_list_with_tab_all_m",
    "previewImg": "http://edu-image.nosdn.127.net/367ae591-32f3-4337-a195-cafd5543082e.png",
    "style": "",
    "type": 2,
    "viewType": 2
}
```



<slide :class="aligncenter">

### 数据元数据

``` json
{
  "name" : "c2_voteList",
  "description" : "投票列表模板",
  "items" : [
    {
      "description" : "投票列表类型,10为机构,20为讲师",
      "type" : "Number",
      "propertyName" : "id"
    },{
      "description" : "一页数量",
      "type" : "Number",
      "propertyName" : "nums"
    },{
      "description" : "投票结束时间",
      "type" : "Time",
      "propertyName" : "endTime"
    }
  ]
}

```



<slide class="bg-black aligncenter" image="http://edu-image.nosdn.127.net/f07f5cb8e4e94ea4be7d845ead5f29d8.jpg?imageView&quality=100 .dark">

# 降低重复性工作 {.text-landing.text-shadow}

好好度个假 {.text-intro.animated.fadeInUp.delay-500}

[:fa-github: Github](https://github.com/ksky521/nodeppt){.button.ghost.animated.flipInX.delay-1200}