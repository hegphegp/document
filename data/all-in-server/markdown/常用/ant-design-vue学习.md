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


### 单个页面居中表单
```
<template>
  <div class="container">
    <div class="main">
      <a-form
        id="formLogin"
        class="user-layout-login"
        ref="formLogin"
        :form="form"
        @submit="handleSubmit"
      >
        <h1 style="text-align: center;">后台管理系统登录页面后台管理系统登录页面后台管理系统登录页面</h1>
        <a-form-item>
          <a-input
            size="large"
            type="text"
            placeholder="请输入账户名或邮箱地址"
            v-decorator="[
            'loginAccount',
            {rules: [{required: true,message:'账户名称不能为空！'},{max:20,message:'账号长度不能超过20个字符！'}]
            , validateTrigger: 'blur'}
            ]"
          >
          </a-input>
        </a-form-item>
        <a-form-item>
          <a-input
            size="large"
            type="password"
            placeholder="请输入密码"
            v-decorator="[
            'loginPassword',
            {rules: [{required: true,message:'密码不能为空！'},{max:20,message:'密码长度不能超过20个字符！'}]
            , validateTrigger: 'blur'}
            ]"
          >
          </a-input>
        </a-form-item>
        <a-form-item>
          <a-col :span="16">
            <a-input
              size="large"
              type="text"
              placeholder="请输入验证码"
              v-decorator="[
              'verifyCode',
              {rules: [{required: true,message:'验证码不能为空！'}]
              , validateTrigger: 'blur'}
              ]"
            >
            </a-input>
          </a-col>
          <a-col :span="7" style="float:right">
            <img src="http://layuimini.99php.cn/iframe/v2/images/captcha.jpg"/>
          </a-col>
        </a-form-item>
        <a-form-item style="margin-top: 24px;">
          <a-button
            size="large"
            type="primary"
            htmlType="submit"
            class="login-button"
          >登陆
          </a-button>
        </a-form-item>
      </a-form>
    </div>
  </div>
</template>
<script>

export default {
  name: 'login',
  data () {
    return {
      form: this.$form.createForm(this)
    }
  },
  methods: {
    handleSubmit (e) {
      e.preventDefault()
      this.form.validateFields(err => {
        console.log('err=>' + err)
      })
    }
  }
}

</script>
<style lang="less" scoped>
  .container {
    width: 100%;
    min-height: 100%;
    background: #f0f2f5 url(../../assets/login.svg);
    background-size: 100%;
    padding: 210px 0 144px;
    position: relative;

    .top {
      text-align: center;

      .header {

        .logo {
          vertical-align: top;
          margin-right: 16px;
          border-style: none;
        }
      }

    }

    .main {
      min-width: 260px;
      width: 368px;
      margin: 0 auto;

      .user-layout-login {
        label {
          font-size: 14px;
        }

        .getCaptcha {
          display: block;
          width: 100%;
          height: 40px;
        }

        .forge-password {
          font-size: 14px;
        }

        button.login-button {
          padding: 0 15px;
          font-size: 16px;
          height: 40px;
          width: 100%;
        }

        .user-login-other {
          text-align: left;
          margin-top: 24px;
          line-height: 22px;

          .item-icon {
            font-size: 24px;
            color: rgba(0, 0, 0, 0.2);
            margin-left: 16px;
            vertical-align: middle;
            cursor: pointer;
            transition: color 0.3s;

            &:hover {
              color: #1890ff;
            }
          }

          .register {
            float: right;
          }
        }
      }

    }
  }

</style>

```