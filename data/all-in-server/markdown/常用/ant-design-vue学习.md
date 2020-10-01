#### ant-design-vue学习

### 1. 命令行创建项目

#### 1.1. 命令行创建项目的具体步骤

* 安装vue脚手架
```
npm install -g @vue/cli --verbose
npm install -g @vue/cli-init --verbose
```
* 创建项目
```
vue init webpack hgp-vue-design-vue
```
* 安装依赖
```
vue install --verbose
```
* 运行项目
```
npm run dev
```
* 在package.json添加dependencies，不要用手动输入，用npm install ant-design-vue命令执行，除非使用旧版本的插件
```
# "ant-design-vue":"^1.4.12"
npm install --save ant-design-vue --verbose
```

* 在src/main.js引入 ant-design-vue

```
import 'ant-design-vue/dist/antd.css'        # 在src/main.js引入，属于全局引入，在其他js文件里面，可以直接使用ant-design-vue的标签
import Antd from 'ant-design-vue'    # 在src/main.js引入，属于全局引入，在其他js文件里面，可以直接使用ant-design-vue的标签

Vue.use(Antd)
```

* 在 src/App.vue 添加router-view标签，否则路由页面都不生效，在浏览器输入地址加路由，不会跳转
```
<a-locale-provider>
  <div id="app">
    <router-view/>
  </div>
</a-locale-provider>
```

* 限制 style 只在当前文件生效
```
/**
 * Add "scoped" attribute to limit CSS to this component only
 */
<style scoped></style>
```

* 引入插件

```
npm install --save vue-router --verbose
npm install --save vue-i18n --verbose
npm install --save vuex --verbose
npm install --save less-loader@4.1.0 --verbose
# npm install --save less-loader --verbose
npm install --save less --verbose
npm install --save axios --verbose
npm install --save mockjs --verbose
npm install --save axios-mock-adapter --verbose

```

* npm install -save 和 -save-dev 区别
```
npm install packageName //本地安装，安装到项目目录下，不在package.json中写入依赖
npm install packageName -g //全局安装，安装在Node安装目录下的node_modules下
npm install packageName --save //安装到项目目录下，并在package.json文件的dependencies中写入依赖，简写为-S
npm install packageName --save-dev //安装到项目目录下，并在package.json文件的devDependencies中写入依赖，简写为-D
```

* new Vuex.Store({}) 自定义的方法如何引用

```
import {mapActions} from 'vuex'

export default {
  methods: {
    ...mapActions([
      'setUserLanguage'
    ]),
    handleChange (value) {
      this.setUserLanguage({userLang: value})
    }
  }
}
```


* ESlint+VSCode自动格式化
* * 在Vue.js项目中使用的是eslint检查。每次写代码一不注意就会引起报错，项目运行不了，提示错误信息。
* * 在vscode里只要安装ESlint插件，写完代码保存之后根据ESlint语法要求自动格式化。

```
# 设置setting.json
File -> Preference -> Settings，点击 Edit in setting.json，添加下面的代码

"editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
},
# 配置完之后，VSCode 会根据你当前 Vue 项目下的 .eslintrc.js 文件的规则来验证和格式化代码。
```

* 基于vue-router从后端动态加载菜单的实现

```
```

### 下面抛的错让我痛不欲生，让我无尽绝望，对于一个入门学习的人，任何一个错误都会让人绝望
* UnhandledPromiseRejectionWarning: TypeError: loaderContext.getResolve is not a function
* * 构建Vue 项目 安装各种插件，直到安装less，编译就卡死在17%进度不动，报错了。检查./build/webpack.base.conf.js配置是否配置有误，再或者是删除node_modules 结果还是报错。。。记得以前安装都是直接使用的。
* * 静下心看是哪一步操作有误，检查一番后发现不是配置问题。百度看了一下别人响应，最后得出的结果就是 less-loader的版本过高，然后直接安装一个npm install less-loader@4.1.0 -s进行覆盖。原先版本是多少也没注意看，是直接安装的，属于最高版本。
