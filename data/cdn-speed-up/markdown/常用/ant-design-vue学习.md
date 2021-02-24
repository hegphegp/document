#### ant-design-vue学习

##### vue-cli4.0快速搭建一个项目，不要使用 vue init webpack 创建，使用 vue create 指令创建项目，否则要一个个在package.json添加依赖包
* devDependencies与dependencies的区别
* babel 是发布时，将 ES6 代码编译成 ES5 ，那么 babel 就是devDependencies
* Vue项目中vue-router，由于发布之后还是依赖vue-router，所以是dependencies
```
# 安装 @vue/cli
npm install -g @vue/cli
# 查看 @vue/cli 版本号
vue -V
# vue create 指令创建项目，vue create <Project Name> //文件名 不支持驼峰（含大写字母）
vue create template
# 创建完项目后，拷贝两个文件 vue.config.js 和 babel.config.js

# 安装到 devDependencies
npm install --save-dev compression-webpack-plugin@6.0.2 --verbose
npm install --save-dev git-revision-webpack-plugin@3.0.6 --verbose
npm install --save-dev babel-plugin-transform-remove-console@6.9.4 --verbose
npm install --save-dev babel-plugin-import@1.12.2 --verbose

# 安装到 dependencies
npm install --save @ant-design-vue/pro-layout@1.0.1 --verbose
npm install --save axios@0.20.0 --verbose
npm install --save vuex@3.5.1 --verbose
npm install --save moment@2.29.1 --verbose
npm install --save nprogress@1.0.0-1 --verbose
npm install --save store@2.0.12 --verbose
npm install --save vue-clipboard2@0.3.1 --verbose
npm install --save vue-cropper@0.5.5 --verbose
npm install --save ant-design-vue@1.6.5 --verbose
npm install --save vue-router@3.4.6 --verbose

```

### npm查看插件版本列表
```
# npm查看插件版本列表
npm view compression-webpack-plugin versions
# npm安装指定版本的插件
npm install compression-webpack-plugin@6.0.2 --save-dev --verbose

```

### 0. 被一些问题搞死人了
```
import Vue from 'vue'
import Antd from 'ant-design-vue'
import 'ant-design-vue/dist/antd.css'  // antd.css引入在第3行时，运行抛错，把antd.css引入放到 import router from './router' 后面就不抛错了
import App from './App'
import router from './router'

Vue.config.productionTip = false

Vue.use(Antd)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})

# 不反对抛错，但是痛恨抛垃圾错误信息，毫无作用的错误信息，完全把人给带偏了，坑死人了。下面的错误是由于 antd.css引入顺序导致的
hgp@hgp-MS-7B89:~/workspace/nodejs/test$ npm run dev

> test@1.0.0 dev /home/hgp/workspace/nodejs/test
> webpack-dev-server --inline --progress --config build/webpack.dev.conf.js

 13% building modules 30/33 modules 3 active ...orkspace/nodejs/test/src/App.vue{ parser: "babylon" } is deprecated; we now treat it as { parser: "babel" }.
 94% asset optimization                                                                  

 ERROR  Failed to compile with 1 errors                                                                                                                                                                                                  10:34:16 ├F10: AM┤

This dependency was not found:

* ant-design-vue/dist/antd.less in ./src/main.js

To install it, you can run: npm install --save ant-design-vue/dist/antd.less
^Z
[2]+  已停止               npm run dev
hgp@hgp-MS-7B89:~/workspace/nodejs/test


```

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

* 拼接URL参数

```
import qs from 'qs'
let Axios = require('axios');
axios.request({
    url: url,
    data:qs.stringify(params),
    method: 'post'
});

```

* store.commit和store.dispatch的区别及用法

```
# 规范的使用方式：
# 以载荷形式，参数可有可无？
store.commit('increment'，{
  amount: 10   //这是额外的参数
})

# 或者使用对象风格的提交方式
store.commit({
  type: 'increment',
  amount: 10   //这是额外的参数
})



# dispatch：含有异步操作，数据提交至 actions ，可用于向后台提交数据
this.$store.dispatch('isLogin', true);
# commit：同步操作，数据提交至 mutations ，可用于读取用户信息写到缓存里
this.$store.commit('loginStatus', 1);
```

```
router.beforeEach((to, from, next) => {
  iView.LoadingBar.start();
  Util.title(to.meta.title);
  if (to.meta.requireAuth) {
    console.log('需要权限，暂时放开！');
    next();
  } else {
    next();
  }
});

router.afterEach((to) => {
  iView.LoadingBar.finish();
  window.scrollTo(0, 0);
});
```

* Vue子组件向父组件传值（this.$emit()方法）

```
# 子组件 Language.vue
<template>
  <Dropdown trigger="click" @on-click="selectLang">
    ...... 省略
  </Dropdown>
</template>

<script>
export default {
  name: 'Language',
  methods: {
    selectLang (name) {
      this.$emit('on-lang-change', name) // 重点这句话
    }
  }
}
</script>

# 父组件 parent.vue
<div class="layout-nav">
  <language @on-lang-change="setLanguage" style="margin-right: 10px;" :lang="local"/>  // this.$emit('on-lang-change', name) 会把值传给 on-lang-change
</div>

<script>
import Language from '../../components/language';
export default {
  components: {
    Language
  },
  methods: {
    setLanguage(lang) { // setLanguage 接收 on-lang-change 的传值
      localStorage.setItem('lang', lang)
    }
  }
}
</script>
```

* ant-design-vue的form点击提交时，获取表单所有数据

```
<a-form id="formLogin" class="user-layout-login" :form="form" @submit="handleSubmit">
</a-form>
export default {
  components: { },
  data () {
    return {
      form: this.$form.createForm(this)
    }
  },
  methods: {
    handleSubmit (e) {
      e.preventDefault()
      this.form.validateFields({ force: true }, (err, values) => {
        // values是表单的所有值
      })
    }
  }
}
```

* 路由配置
```
1）在导航守卫里面，好像博客说不能使用this对象，因为此时页面还没生成，使用this对象，控制台抛错。
2）后台管理页面的嵌套路由，每一层路由都要有 component 组件（没有component组件，子孙路由的页面不会显示任何内容），第一层根路径一定是 component: BasicLayout 布局，除了最后一层路由是具体的页面 component 外，其他父级路由都是 component: RouteView
```

* 调用 vuex 方法的步骤写法
```
<script>
import { mapActions } from 'vuex'       // 步骤1

export default {
  data () {
    return { }
  },
  methods: {
    ...mapActions(['Login', 'Logout']), // 步骤2
    handleSubmit (e) {
      e.preventDefault()

      this.Login(loginParams)                // 步骤3调用
        .then((res) => this.loginSuccess(res))
        .catch(err => this.requestFailed(err))
        .finally(() => {
          this.state.loginBtn = false
        })
      })
    }
  }
}
</script>
```

* vuex直接修改state 与 用dispatch/commit来修改state的差异

```
# 一. 使用vuex修改state时，有两种方式：
# 1）可以直接使用 this.$store.state.变量 = xxx
# 2）可以直接使用 this.$store.dispatch(actionType, payload)
# 3）可以直接使用 this.$store.commit(commitType, payload)

# 二. 异同点
# 1）共同点： 能够修改state里的变量，并且是响应式的（能触发视图更新）
# 2）不同点：
#      若将vue创建 store 的时候传入 strict: true, 开启严格模式，那么任何修改state的操作，只要不经过 mutation的函数，vue就会 throw error : [vuex] Do not mutate vuex store state outside mutation handlers。
#      使用dispatch 和 commit的区别在于，前者是异步操作，后者是同步操作，所以 一般情况下，推荐直接使用commit，即 this.$store.commit(commitType, payload)，以防异步操作会带来的延迟问题。

# 三.使用commit提交到mutation修改state的优点：
# vuex能够记录每一次state的变化记录，保存状态快照，实现时间漫游/回滚之类的操作。

# 结论： 官方推荐最好设置严格模式，并且每次都要commit来修改state，而不能直接修改state，以便于调试等。
```

* 基于vue-router从后端动态加载菜单的实现

```
```

* this.$notification.success的duration参数设置提示框的显示时长
```
this.$notification.success({
  message: '欢迎',
  description: `欢迎回来`,
  duration: 2
})
```

### 父子组件传值，实现列表打开新增modal框，编辑框，关闭后自动刷新列表
```javascript
<!-- 父子组件传值 -->
<!-- 父组件 -->
<button @onclick="openFormModal('add')">新增</button>
<FormModal ref="formMadal" @refreshPage="refreshPage"></FormModal>
<script type="text/javascript">
    methods: {
      openFormModal (type) {
        this.$refs.formMadal.openDrawer(row);
      },
      refreshPage () {
        // 在此处刷新页面
        this.$refs.table.refresh(true)
      }
    }
</script>

<!-- 子组件 -->
<template>
  <a-modal
    title="新建规则"
    :wkeyth="640"
    :visible="visible"
    :confirmLoading="loading"
    @cancel="handleCancel">
    <a-spin :spinning="loading">

      ... 控件 ...

      <template slot="footer">
        <a-button type="primary" @click="handleOk" :disabled="loading">新增</a-button>
        <a-button type="primary" @click="handleOk" :disabled="loading">保存</a-button>
        <a-button type="primary" @click="handleCancel" :disabled="loading">返回</a-button>
      </template>
    </a-spin>
  </a-modal>
</template>

<script>
export default {
  data () {
    return {
      visible: false,
      form: this.$form.createForm(this)
    }
  },
  methods: {
    openFormModal (type = 'add') {
      this.visible = true
    },
    handleCancel () {
      this.loading = true
      setTimeout(() => {
        this.visible = false
        this.loading = false
        // 通过 this.$emit 加上方法名调用父组件的方法
        this.$emit('refreshPage')
      }, 1000)
    },
    handleOk () {
      this.loading = true
      setTimeout(() => {
        this.visible = false
        this.loading = false
        // 通过 this.$emit 加上方法名调用父组件的方法
        this.$emit('refreshPage')
      }, 1000)
    }
  }
}
</script>
```


### 下面抛的错让我痛不欲生，让我无尽绝望，对于一个入门学习的人，任何一个错误都会让人绝望
* UnhandledPromiseRejectionWarning: TypeError: loaderContext.getResolve is not a function
* * 构建Vue 项目 安装各种插件，直到安装less，编译就卡死在17%进度不动，报错了。检查./build/webpack.base.conf.js配置是否配置有误，再或者是删除node_modules 结果还是报错。。。记得以前安装都是直接使用的。
* * 静下心看是哪一步操作有误，检查一番后发现不是配置问题。百度看了一下别人响应，最后得出的结果就是 less-loader的版本过高，然后直接安装一个npm install less-loader@4.1.0 -s进行覆盖。原先版本是多少也没注意看，是直接安装的，属于最高版本。

### 单个列表页面

```
<template>
  <a-card :bordered="false">
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48" style="margin-left: -6px; margin-right: -6px;">
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="规则编号" :colon="false">
              <a-input v-model="queryParam.id" placeholder=""/>
            </a-form-item>
          </a-col>
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="使用状态" :colon="false">
              <a-select v-model="queryParam.status" placeholder="请选择" default-value="0">
                <a-select-option value="0">全部</a-select-option>
                <a-select-option value="1">关闭</a-select-option>
                <a-select-option value="2">运行中</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="调用次数" :colon="false">
              <a-input-number v-model="queryParam.callNo" style="width: 100%"/>
            </a-form-item>
          </a-col>
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="年月日" :colon="false">
              <a-date-picker v-model="queryParam.yearMonthDay" style="width: 100%" placeholder="请输入更新日期"/>
            </a-form-item>
          </a-col>
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="年月日时分秒" :colon="false">
              <a-date-picker v-model="queryParam.yyyyMMddHHmmss" format="YYYY-MM-DD HH:mm:ss" show-time style="width: 100%" placeholder="请输入更新日期"/>
            </a-form-item>
          </a-col>
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="年月日时分秒" :colon="false">
              <a-date-picker
                v-model="queryParam.yyyyMMddHHmmss1"
                style="width: 100%"
                placeholder="请输入更新日期"
                format="YYYY-MM-DD HH:mm:ss"
                show-time
                :disabled-date="disabledDate"
                :disabled-time="disabledRangeTime"/>
            </a-form-item>
          </a-col>
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="年月" :colon="false">
              <a-month-picker v-model="queryParam.yearMonth" style="width: 100%" placeholder="请输入更新日期"/>
            </a-form-item>
          </a-col>
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="使用状态222" :colon="false">
              <a-select v-model="queryParam.useStatus11" placeholder="请选择" default-value="0">
                <a-select-option value="0">全部</a-select-option>
                <a-select-option value="1">关闭</a-select-option>
                <a-select-option value="2">运行中</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :lg="6" :md="8" :sm="24" style="padding-left: 6px; padding-right: 6px;">
            <a-form-item label="下拉框默认值" :colon="false">
              <a-select v-model="queryParam.selectValue" showSearch placeholder="请选择">
                <a-select-option v-for="item in selectDatas" :key="item.value" :value="item.value"> {{ item.text }} </a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-button style="margin-bottom: 8px; margin-left: 8px" @click="resetQueryParams()">重置</a-button>
          <a-button style="margin-bottom: 8px; margin-left: 8px" type="primary" @click="$refs.table.refresh(true)">查询</a-button>
          <a-button style="margin-bottom: 8px; margin-left: 8px" type="primary" @click="$refs.table.refresh(true)">查询</a-button>
          <a-button style="margin-bottom: 8px; margin-left: 8px" type="primary" icon="plus">新建</a-button>
        </a-row>
      </a-form>
    </div>

    <s-table ref="table" size="default" rowKey="key"
      :columns="columns"
      :data="loadData"
      :showAlert="false"
      :alert="true"
      :rowSelection="rowSelection"
      :pagination="pagination"
      bordered>
      <a slot="name" slot-scope="text">{{ text }}</a>
      <span slot="customTitle"><a-icon type="smile-o" /> Name</span>
      <span slot="tags" slot-scope="tags">
        <a-tag v-for="tag in tags" :key="tag"
          :color="tag === 'loser' ? 'volcano' : tag.length > 5 ? 'geekblue' : 'green'">
          {{ tag.toUpperCase() }}
        </a-tag>
      </span>
      <span slot="action" slot-scope="text, record">
        <a>Invite 一 {{ record.name }}</a>
        <a-divider type="vertical" />
        <a>Delete</a>
        <a-divider type="vertical" />
        <a class="ant-dropdown-link"> More actions <a-icon type="down" /> </a>
      </span>
    </s-table>
  </a-card>
</template>

<script>
import STable from '@/components/Table'
import moment from 'moment'

const columns = [
  { dataIndex: 'name', key: 'name', slots: { title: 'customTitle' }, scopedSlots: { customRender: 'name' } },
  { title: 'Age', dataIndex: 'age', key: 'age', scopedSlots: { customRender: 'age' } },
  { title: 'Address', dataIndex: 'address', key: 'address', scopedSlots: { customRender: 'address' } },
  { title: 'Tags', key: 'tags', dataIndex: 'tags', scopedSlots: { customRender: 'tags' } },
  { title: 'Action', key: 'action', scopedSlots: { customRender: 'action' } }
]

const data = {
  pageSize: 5,
  pageNo: 1,
  totalCount: 101,
  totalPage: 21,
  data: [
    { key: '0', name: 'Joe Black', age: 32, address: 'Sidney No. 1 Lake Park', tags: ['cool', 'teacher'] },
    { key: '1', name: 'Joe Black', age: 32, address: 'Sidney No. 1 Lake Park', tags: ['cool', 'teacher'] },
    { key: '2', name: 'Joe Black', age: 32, address: 'Sidney No. 1 Lake Park', tags: ['cool', 'teacher'] },
    { key: '3', name: 'Joe Black', age: 32, address: 'Sidney No. 1 Lake Park', tags: ['cool', 'teacher'] },
    { key: '4', name: 'Joe Black', age: 32, address: 'Sidney No. 1 Lake Park', tags: ['cool', 'teacher'] }
  ]
}

export default {
  name: 'TestList',
  components: {
    STable
  },
  data () {
    this.columns = columns
    return {
      pagination: {
        showTotal: total => `共 ${total} 条数据`,
        pageSizeOptions: ['5, '10', '20', '50', '100']
      },
      selectDefaultValue: null,
      defaultSearchTimeValue: null,
      // 下拉框数据的对象
      selectDatas: [],
      // confirmLoading: false,
      mdl: null,
      // 查询参数
      queryParam: { },
      // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        const requestParameters = Object.assign({}, parameter, this.queryParam)
        if (this.queryParam.yearMonthDay != null && this.queryParam.yearMonthDay !== undefined) {
          console.log(this.queryParam.yearMonthDay.valueOf())
        }
        console.log(JSON.stringify(requestParameters))
        data.pageNo = parameter.pageNo
        return new Promise((resolve, reject) => {
          // 1. 模拟一个异步请求，想要将成功的数据发送出去
          // 2. 将成功的数据放在resolve函数中，传递出去
          // setTimeout(() => {
          //   resolve(data)
          // }, 200)
          resolve(data)
        }).then(data => {
          console.log(JSON.stringify(data))
          return data
        }).catch(err => {
          console.log(err)
        })
      },
      selectedRowKeys: [],
      selectedRows: []
    }
  },
  // created 初始从后端加载下拉框数据
  created () {
    this.initDefaultValues()
  },
  // 添加加载下拉框数据的方法
  methods: {
    disabledDate (current) {
      // 限制不允许选择昨天
      const yesterday = moment(new Date()).add(-1, 'days')
      return current && current < yesterday
      // return false
    },
    disabledRangeTime () {
      return {
        disabledHours: () => [-1, 24],
        disabledMinutes: () => [-1, 60],
        disabledSeconds: () => [-1, 60]
      }
    },
    initDefaultValues () {
      this.defaultSearchTimeValue = 1596471447000
      if (this.defaultSearchTimeValue != null && this.defaultSearchTimeValue !== undefined) {
        this.$set(this.queryParam, 'dateValue', moment(this.defaultSearchTimeValue))
      }
      this.initSelectDatas()
    },
    initSelectDatas () {
      return new Promise((resolve, reject) => {
        // 1. 模拟一个异步请求，想要将成功的数据发送出去
        // 2. 将成功的数据放在resolve函数中，传递出去
        // setTimeout(() => {
        //   resolve(data)
        // }, 200)
        const data = [
          { code: 'ALL', name: '全部' },
          { code: 'STATUS1', name: '状态1' },
          { code: 'STATUS2', name: '状态2' },
          { code: 'STATUS3', name: '状态3' }
        ]
        resolve(data)
      }).then(data => {
        data.forEach((item) => {
          this.selectDatas.push({
            value: item.code,
            text: item.name
          })
        })
        // 动态给下拉框配置默认值，下拉框的数据源长度大于3时，才给默认值
        if (this.selectDatas.length > 3) {
          this.selectDefaultValue = this.selectDatas[2].value
          this.$set(this.queryParam, 'selectValue', this.selectDefaultValue)
        }
        return data
      }).catch(err => {
        console.log(err)
      })
    },
    resetQueryParams () {
      this.queryParam = {}
      if (this.selectDefaultValue != null && this.selectDefaultValue !== undefined) {
        this.$set(this.queryParam, 'selectValue', this.selectDefaultValue)
      }
      if (this.defaultSearchTimeValue != null && this.defaultSearchTimeValue !== undefined) {
        this.$set(this.queryParam, 'dateValue', moment(this.defaultSearchTimeValue))
      }
    }
  },
  computed: {
    rowSelection () {
      return {
        selectedRowKeys: this.selectedRowKeys,
        onChange: this.onSelectChange
      }
    }
  }
}
</script>
```

### 单个页面居中表单
```
<template>
  <div class="container">
    <div class="main">
      <a-form id="formLogin" class="user-layout-login" ref="formLogin" :form="form" @submit="handleSubmit">
        <h1 style="text-align: center;">后台管理系统登录页面后台管理系统登录页面后台管理系统登录页面</h1>
        <a-form-item>
          <a-input size="large" type="text" placeholder="请输入账户名或邮箱地址"
            v-decorator="[
            'loginAccount',
            {rules: [{required: true,message:'账户名称不能为空！'},{max:20,message:'账号长度不能超过20个字符！'}]
            , validateTrigger: 'blur'}
            ]"
          >
          </a-input>
        </a-form-item>
        <a-form-item>
          <a-input size="large" type="password" placeholder="请输入密码"
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
            <a-input size="large" type="text" placeholder="请输入验证码"
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
          <a-button size="large" type="primary" htmlType="submit" class="login-button">登陆</a-button>
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

##### 同步，异步执行
##### 使用了await之后，await外层的方法生命一定要加async关键字，这是背诵默写的规则
* async修饰方法调用，并行执行
* await修饰方法调用，串行执行

```
async getBooksAndAuthor(authorId) {
    const books = await bookModel.fetchAll();
    const author = await authorModel.getch(authorId);
    return {
        author,
        books: books.filter(book => book.aurhotId === authorId)
    }
}


async getBooksAndAuthor(authorId) {
    const bookPromise = bookModel.fetchAll();
    const authorPromise = authorModel.getch(authorId);
    const book = await bookPromise;
    const author = await authorPromise;
    return {
        author,
        books: books.filter(book => book.aurhotId === authorId)
    }
}

```