# 项目开发流程

## 1. 创建客户端项目

###  1.1 使用 vue-cli(脚手架)搭建项目 

```bash
#在Github新建Vue-MintShop项目,然后clone到本地
git clone git@github.com:W-Qing/Vue-MintShop.git
cd Vue-MintShop
#创建客户端项目
vue init webpack mintshop-client
cd mintshop-client
npm install
npm run dev 访问: localhost:8080
```

### 1.2 项目结构分析 

>  **MintShop-client **
>
>  - |-- build : webpack 相关的配置文件夹(基本不需要修改) 
>  - |-- build : webpack 相关的配置文件夹(基本不需要修改) 
>  - |-- config: webpack 相关的配置文件夹(基本不需要修改)
>  - |-- index.js: 指定的后台服务的端口号和静态资源文件夹 
>  - |-- node_modules 
>  - |-- src : 源码文件夹 
>  - |-- main.js: 应用入口 js  （初始化vue实例并使用需要的插件 ）
>  - |-- static: 静态资源文件夹
>  - |-- .babelrc: babel 的配置文件
>  - |-- .editorconfig: 通过编辑器的编码/格式进行一定的配置
>  - |-- .eslintignore: eslint 检查忽略的配置
>  -  |-- .eslintrc.js: eslint 检查的配置 
>  - |-- .gitignore: git 版本管理忽略的配置 
>  - |-- index.html: 默认的主渲染页面文件 
>  - |-- package.json: 应用包配置文件
>  - |-- README.md: 应用描述说明的 readme 文件 

### 1.3 编码测试与打包发布项目  

- 编码测试 

  npm run dev 

  访问: http://localhost:8080 

  编码, 自动编译打包(HMR), 查看效果 

- 打包发布 

  npm run build 

  npm install -g serve 

  serve dist

  访问: http://localhost:5000  

## 2. 功能需求分析

- 开发前应该首先完成功能模块的分析设计，这里我们可以直接运行项目查看功能演示 自己总结项目功能需求

## 3. 开发资源准备
- 使用[阿里巴巴矢量库]( http://www.iconfont.cn/)
- 将想要的图标添加入库（购物车） 
- 将购物车中的图标添加到项目中 
- 生成项目图标的[Font Class地址](//at.alicdn.com/t/font_726904_68moeoeirj3.css)

## 4. Css Reset、Fastclick与Stylus

### 4.1 Css Reset

- 在项目主目录下的static文件夹内新建css文件夹

- 在css文件夹内新建重置样式文件reset.css 

- 在index.html 中引入 

  ```html
  <link rel="stylesheet" href="/static/css/reset.css">
  ```

### 4.2 Fastclick

> 当用户一次点击屏幕之后，浏览器并不能立刻判断用户是要进行双击缩放，还是想要进行单击操作。因此，iOS Safari 就等待 300 毫秒，以判断用户是否再次点击了屏幕。 于是，300 毫秒延迟就这么诞生了。

- 安装fastclick库  解决点击响应延时 0.3s 问题 

  ```bash
  npm Install fastclick --save
  ```

- 在main.js中引入，并绑定到body

  ```js
  import FastClick from 'fastclick'
  FastClick.attach(document.body);
  ```

### 4.3 Stylus

- 安装stylus依赖包

  stylus-loader要安装指定版本，否则版本过高会报错

  ```npm i stylus-loader@3.0.2 -S
  npm i stylus-loader@3.0.2 -S
  ```

  ```bash
  npm install stylus --save-dev
  ```

- 在common文件夹下新建stylus文件夹

- 在stylus文件加下面新建mixins.styl文件

- 注意在组件内编写样式时要声明lang和rel

  ```html
  <style lang="stylus" rel="stylesheet/stylus">
  ```

## 5. 源码目录设计

![src目录结构](http://owoccema2.bkt.clouddn.com/Readme/vue/mintshop.png)

## 6. Vue组件化

### 6.1 分析整个项目的 vue 组件结构 

> **src**
> - |-- components------------非路由组件文件夹 
> - |-- FooterGuide---------------底部组件文件夹 
>   - |-- FooterGuide.vue--------底部组件 vue 
> - |-- pages-----------------路由组件文件夹 
>   - |-- Msite---------------首页组件文件夹 
>     - |-- Msite.vue--------首页组件 vue
>   - |-- Search----------------搜索组件文件夹 
>     - |-- Search.vue---------搜索组件 vue 
>   - |-- Order--------------订单组件文件夹
>     - |-- Order.vue-------订单组件 vue
>   - |-- Profile--------------个人组件文件夹 
>     - |-- Profile.vue-------个人组件 vue 
> - |-- App.vue---------------应用根组件 vue 
> - |-- main.js---------------应用入口 js 

1. 页面底部的FooterGuide组件只是用来放pages里的组件的容器，所以它不是路由组件
2. 页面最上面的标题栏在我们的项目中属于路由组件的组成部分（与中间内容部分在一起）
3. 但每个路由组件中都有最顶部的组件且相似度很高，所以可以将其抽取成为一个单独的组件

### 6.2 编写vue组件模板文件

- pages文件夹下的各个vue组件文件及App.vue和FooterGuide.vue都是这个初始空白模板

```vue
<template>
  <div>App vue template</div>
</template>

<script>
  export default {}
</script>

<style lang="stylus" rel="stylesheet/stylus">
</style>
```

## 7. 引入Vue-router

###7.1 下载vue-router 

   ```bash
#创建项目时已下载
npm install vue-router --save
   ```

### 7.2 编写router文件夹下的index.js

```javascript
 /*
   路由模块 
 */
 import Vue from 'vue'
 import VueRouter from 'vue-router'
 // 引入路由组件文件夹下的组件
 import Msite from '../pages/Msite/Msite.vue'
 import Search from '../pages/Search/Search.vue'
 import Order from '../pages/Order/Order.vue'
 import Profile from '../pages/Profile/Profile.vue'
 // 全局注册Vue-router组件
 Vue.use(VueRouter)
 
 // 配置路由表并导出
 export default new VueRouter({
   //去掉地址中的哈希#
   mode: 'history',
   routes: [{
       path: '/',
       redirect: '/msite'
     },
     {
       path: '/msite',
       component: Msite,
     },
     {
       path: '/search',
       component: Search,
     },
     {
       path: '/order',
       component: Order,
     },
     {
       path: '/profile',
       component: Profile,
     }
   ]
 })
```

### 7.3 编写应用的入口文件main.js

```javascript
// 引入路由 其实就是引入上一步配置好的路由表
import router from './router'

new Vue({
  el: '#app',
  render: h => h(app),
  // 为根组件加入路由
  router
})
```

### 7.4 在App.vue里使用router-view

```vue
<template>
  <!-- 修改应用组件的模板 -->
  <div id="app">
    <!-- 设置路由组件的视图位置 -->
    <router-view></router-view>
    <!-- 并放置非路由组件 -->
    <FooterGuide></FooterGuide>
  </div>
</template>

<script>
import FooterGuide from './components/FooterGuide/FooterGuide.vue'
// 引入底部组件并注册
export default {
  components: {
    FooterGuide
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
// 整个应用组件的样式
  #app
    width 100%
    height 100%
    background #f5f5f5
</style>
```

- App.vue里的根元素`<div id="app"></div>`与外层被注入框架index.html中的`<div id="app"></div>`是一致的 
- index.html中的`<div id="app"></div>`是指定绑定目标为元素的根路径，而App.vue文件里的`<div id="app"></div>`则是提供注入绑定元素的内容，两者在运行时指的是同一个DOM元素 

### 7.5 运行并请求不同路由路径查看效果

通过切换url地址里的hash值（miste/order/search/profile），页面会显示不同的路由模板内容。

## 8. 编写组件代码

### 8.1 FooterGuide组件

**功能及实现**

1. 通过编程式导航实现路由的切换显示($router) 
2. 通过动态 class 和$route.path 来实现 tab 样式切换 
3. 通过阿里图标库, 显示导航图标 
4. 可以用```<router-link>```实现

**代码**

```html
<footer class="footer_guide border-1px">
    <a href="javascript:;" class="guide_item on">
    <span class="item_icon">
    <i class="iconfont icon-food"></i>
    </span>
    <span>外卖</span>
    </a>
    <!--其他三部分类似-->
</footer>
```

```css
<style lang="stylus" rel="stylesheet/stylus">
/*引入公共样式*/
@import "../../common/stylus/mixins.styl"
.footer_guide
  /*顶部有白色的边框*/
  top-border-1px(#e4e4e4)
  position fixed
  z-index 100
  left 0
  right 0
  bottom 0
  background-color #fff
  width 100%
  height 50px
  display flex
  .guide_item
    display flex
    flex 1
    text-align center
    flex-direction column
    align-items center
    margin 5px
    color #999999
    &.on
      color #02a774
    span
      font-size 12px
      margin-top 2px
      margin-bottom 2px
      .iconfont
        font-size 22px
</style>
```

此时，页面已达到理想效果。接着修改template模板，为其加入路由与样式的切换控制。

```html
<div class="guide_item" @click="goto('/msite')" :class="{on: isCurrent('/msite')}">
  <span class="item_icon">
  <i class="iconfont icon-food"></i>
  </span>
  <span>首页</span>
</div>
<!--其他三个部分类似，只是路由不同-->
```

再补充相应的函数方法

```javascript
export default {
  methods: {
    goto (path) {
      this.$router.replace(path)
    },
    isCurrent (path) {
      // console.log(this.$route.path)
      return this.$route.path === path
    }
  }
}
```

至此，底部组件完成，可实现点击不同的选项切换不同的路由组件。

### 8.2 各导航路由组件

#### 8.2.1 Msite组件

**功能区域划分**

1. 最顶部的title标题栏部分
2. 上方的nav轮播区域
3. 商家列表展示区域

**图片资源**

Msite组件页面的轮播图及商家列表都需要用到一些图片资源文件，所以在msite.vue同级目录下新建images文件夹，以便放置各种不同类型的图片资源。

**代码**

```vue
<template>
  <section class="msite">
    <!--首页头部title-->
    <!--由msite_header改成header-->
    <header class="header">
      <span class="header_search">
        <i class="iconfont icon-sousuo"></i>
      </span>
      <span class="header_title">
        <span class="header_title_text ellipsis">芝罘区鲁东大学北区(青年南路)</span>
      </span>
      <span class="header_login">
        <span class="header_login_text">登录|注册</span>
      </span>
    </header>
    <!--首页导航轮播-->
    <nav class="msite_nav">
      <div class="swiper-container">
        <div class="swiper-wrapper">
          <div class="swiper-slide">
            <a href="javascript:" class="link_to_food">
              <div class="food_container">
                <img src="./images/nav/1.jpg">
              </div>
              <span>甜品饮品</span>
            </a>
            <a href="javascript:" class="link_to_food">
              <div class="food_container">
                <img src="./images/nav/2.jpg">
              </div>
              <span>商超便利</span>
            </a>
           <!--下面的图片省略-->
          </div>
          <div class="swiper-slide">
            <a href="javascript:" class="link_to_food">
              <div class="food_container">
                <img src="./images/nav/9.jpg">
              </div>
              <span>甜品饮品</span>
            </a>
            <!--同样省略-->
          </div>
        </div>
        <!-- 轮播图页码 -->
        <div class="swiper-pagination"></div>
      </div>
    </nav>
    <!--首页附近商家列表-->
    <div class="msite_shop_list">
      <div class="shop_header">
        <i class="iconfont icon-xuanxiang"></i>
        <span class="shop_header_title">附近商家</span>
      </div>
      <div class="shop_container">
        <ul class="shop_list">
          <li class="shop_li border-1px">
            <a>
              <div class="shop_left">
                <img class="shop_img" src="./images/shop/1.jpg">
              </div>
              <div class="shop_right">
                <section class="shop_detail_header">
                  <h4 class="shop_title ellipsis">锄禾日当午，汗滴禾下土</h4>
                  <ul class="shop_detail_ul">
                    <li class="supports">保</li>
                    <li class="supports">准</li>
                    <li class="supports">票</li>
                  </ul>
                </section>
                <section class="shop_rating_order">
                  <section class="shop_rating_order_left">
                    <div class="star star-24">
                      <span class="star-item on"></span>
                      <span class="star-item on"></span>
                      <span class="star-item on"></span>
                      <span class="star-item half"></span>
                      <span class="star-item off"></span>
                    </div>
                    <div class="rating_section">
                      3.6
                    </div>
                    <div class="order_section">
                      月售106单
                    </div>
                  </section>
                  <section class="shop_rating_order_right">
                    <span class="delivery_style delivery_right">硅谷专送</span>
                  </section>
                </section>
                <section class="shop_distance">
                  <p class="shop_delivery_msg">
                    <span>¥20起送</span>
                    <span class="segmentation">/</span>
                    <span>配送费约¥5</span>
                  </p>
                </section>
              </div>
            </a>
          </li>
          <!--省略其他店铺展示-->
        </ul>
      </div>
    </div>
  </section>
</template>
<!--省略js-->
```

**要注意首页的头部标题部分的样式，在其他的组件中都可以进行重用。**所以将header标签的类名由msite_header改成header。接下来在其他组件中可以直接使用（当然header里的部分样式其他组件用不到，到时再进一步抽取公共的css样式。）

```css
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixins.styl"
  .msite  //首页
    width 100%
    .header
      background-color #02a774
      position fixed
      z-index 100
      left 0
      top 0
      width 100%
      height 45px
      .header_search
        position absolute
        left 15px
        top 50%
        transform translateY(-50%)
        width 10%
        height 50%
        .icon-sousuo
          font-size 25px
          color #fff
      .header_title
        position absolute
        top 50%
        left 50%
        transform translate(-50%, -50%)
        width 50%
        color #fff
        text-align center
        .header_title_text
          font-size 20px
          color #fff
          display block
      .header_login
        font-size 14px
        color #fff
        position absolute
        right 15px
        top 50%
        transform translateY(-50%)
        .header_login_text
          color #fff
      /*下面的样式省略*/
</style>
```

接下来的几个路由组件都类似，都是先修改template模版，然后引入mixins.styl 样式文件和上面提到的公共的header部分的样式。

#### 8.2.2 Search组件

```vue
<section class="search">
    <header class="header">
      <div class="header_title">
        <span class="header_title_text">搜索</span>
      </div>
    </header>
    <form class="search_form" action="#">
      <input type="search" name="search" placeholder="请输入商家或美食名称"
    class="search_input">
      <input type="submit" class="search_submit">
    </form>
</section>
<!--省略js与style-->
```

#### 8.2.3 Order组件

在order.vue同级目录下新建images文件夹，再新建order文件夹，存放订单组件用到的图片资源。

```vue
<div>
  <section class="order">
    <header class="header">
      <a class="header_title">
        <span class="header_title_text">订单列表</span>
      </a>
    </header>
    <section class="order_no_login">
        <img src="./images/order/person.png">
        <h3>登录后查看外卖订单</h3>
      <button>立即登陆</button>
    </section>
  </section>
</div>
<!--省略js与style-->
```

#### 8.2.4 Profile组件

```vue
<div>
    <section class="profile">
      <header class="header">
        <a class="header_title">
          <span class="header_title_text">我的</span>
        </a>
      </header>
      <section class="profile-number">
        <a href="javascript:" class="profile-link">
          <div class="profile_image">
            <i class="iconfont icon-yonghuming"></i>
          </div>
          <div class="user-info">
            <p class="user-info-top">登录/注册</p>
            <p>
              <span class="user-icon">
                <i class="iconfont icon-msnui-tel icon-mobile"></i>
              </span>
              <span class="icon-mobile-number">暂无绑定手机号</span>
            </p>
          </div>
          <span class="arrow">
            <i class="iconfont icon-previewright"></i>
          </span>
        </a>
      </section>
      <section class="profile_info_data border-1px">
        <ul class="info_data_list">
          <a href="javascript:" class="info_data_link">
            <span class="info_data_top"><span>0.00</span>元</span>
            <span class="info_data_bottom">我的余额</span>
          </a>
          <a href="javascript:" class="info_data_link">
            <span class="info_data_top"><span>0</span>个</span>
            <span class="info_data_bottom">我的优惠</span>
          </a>
          <a href="javascript:" class="info_data_link">
            <span class="info_data_top"><span>0</span>分</span>
            <span class="info_data_bottom">我的积分</span>
          </a>
        </ul>
      </section>
      <section class="profile_my_order border-1px">
        <!-- 我的订单 -->
        <a href='javascript:' class="my_order">
          <span>
            <i class="iconfont icon-dingdan"></i>
          </span>
          <div class="my_order_div">
            <span>我的订单</span>
            <span class="my_order_icon">
              <i class="iconfont icon-previewright"></i>
            </span>
          </div>
        </a>
        <!-- 积分商城 -->
        <a href='javascript:' class="my_order">
          <span>
            <i class="iconfont icon-jifen"></i>
          </span>
          <div class="my_order_div">
            <span>积分商城</span>
            <span class="my_order_icon">
              <i class="iconfont icon-previewright"></i>
            </span>
          </div>
        </a>
        <!-- Mint外卖会员卡 -->
        <a href="javascript:" class="my_order">
          <span>
            <i class="iconfont icon-viptehuishiduan"></i>
          </span>
          <div class="my_order_div">
            <span>Mint外卖会员卡</span>
            <span class="my_order_icon">
              <i class="iconfont icon-previewright"></i>
            </span>
          </div>
        </a>
      </section>
      <section class="profile_my_order border-1px">
        <!-- 服务中心 -->
        <a href="javascript:" class="my_order">
          <span>
            <i class="iconfont icon-lianxikefu"></i>
          </span>
          <div class="my_order_div">
            <span>服务中心</span>
            <span class="my_order_icon">
              <i class="iconfont icon-previewright"></i>
            </span>
          </div>
        </a>
      </section>
    </section>
  </div>
<!--省略js与style-->
```

###  8.3 HeaderTop组件

​	具名 slot 放置不同html

​	props 接收属性

#### 8.3.1组件的构成分析

1. 中间有一个固定的标题栏，只是用在不同的路由组件中显示的内容不同
2. 标题栏两侧可能有搜索框之类的部分（Msite）也可能没有
3. 此组件为非路由组件的公用组件（所以在Components文件夹内）

#### 8.3.2 功能实现的技术

- 标题栏两侧是否有其他部分，要用到slot插槽进行组件间通信 
- slot 通信是标签, 而不是单纯的数据 
- 中间标题栏接收的文本可以用props

#### 8.3.3 组件代码

```vue
<template>
  <header class="header">
    <!-- 插槽是父组件与子组件的通讯方式，子组件中的slot可以显示父组件传递给子组件的内容 -->
    <slot name="left"></slot>
    <span class="header_title">
      <span class="header_title_text ellipsis">{{title}}</span>
    </span>
    <slot name="right"></slot>
  </header>
</template>

<script>
  export default {
    // 外部组件传递给此组件的属性
    props: {
      title: String
    }
  }
</script>
```

#### 8.3.4 在路由组件中使用

- 在要使用此头部组件的文件中引入并注册HeaderTop组件

```javascript
//Msite、Order、Search、Profile中都要引入注册才能使用
import HeaderTop from '../../components/HeaderTop/HeaderTop.vue'
export default {
  components: {
    HeaderTop
  }
}
```

- 然后使用`<HeaderTop></HeaderTop>`标签设置这个头部组件

  > 这里以Msite.vue为例，先删除静态模版里的Header部分，替换成HeaderTop组件

```vue
<!-- 使用 title 来给头部组件传递数据 -->
<HeaderTop title="芝罘区鲁东大学北区(青年南路)">
  <!-- 要使用slot="left"指定插入的插槽位置 -->
  <span class="header_search" slot="left">
    <i class="iconfont icon-sousuo"></i>
      </span>
  <span class="header_login" slot="right">
    <span class="header_login_text">
          登录|注册
    </span>
  </span>
</HeaderTop>
```

- 在其他几个组件中的用法是一样的，同时还省去了slot插槽部分。

### 8.4 使用swiper插件实现轮播图

**下载安装：** 

​	在 package.json 中的 dependencies 加入```"swiper": "^4.2.6",```，否则版本不对，然后`npm install `

**Msite.vue的HTML部分：**

```html
<!--在页面msite_nav导航部分使用swiper-->
<div class="swiper-container">
	<div class="swiper-wrapper">
        <div class="swiper-slide">1</div>
        <div class="swiper-slide">2</div>
        <div class="swiper-slide">3</div>
    </div>
    <!-- swiper轮播图圆点 -->
    <div class="swiper-pagination"></div>
</div>
```

**script部分引入并初始化：**

```javascript
<script>
import Swiper from 'swiper'
//同时引入swiper的 css文件
import 'swiper/dist/css/swiper.min.css'
export default {
  //注意要在页面加载完成之后（mounted）再进行swiper的初始化
  mounted () {
    //创建一个swiper实例来实现轮播
    new Swiper('.swiper-container', {
      autoplay: true,
      // 如果需要分页器
      pagination: {
        el: '.swiper-pagination',
        clickable: true
      }
   })
  }
}
</script>
```
**具体用法参考**[Swiper官方文档](http://www.swiper.com.cn/api/)

### 8.5 拆分出商家列表ShopList组件

1. 商家列表是位于首页轮播图下面的部分，可以拆分为一般组件
2. 在components文件夹下新建ShopList文件夹并新建ShopList.vue文件
3. 该组件中用到需要一些图片资源，所以在ShopList文件夹下还需要新建images文件夹
4. 将原本放在Msite文件夹下的shop和stars图片资源移动到新建的images文件夹内   (其实都应该是动态地从后台获取)
5. 将Msite.vue模板中的`<div class="shop_container"></div> `部分及相应的stylus样式代码移动到新建的ShopList.vue组件
6. 注意还要引入公共的css代码mixins.styl 
7. 最后在Msite.vue中import引入商家列表组件并注册使用

### 8.6 Login组件

**资源文件准备**

1. 登录组件为一级路由组件，所以在pages文件夹下新建Login文件夹和Login.vue
2. template模板里会用到一张svg图片(静态的验证码图片)，所以还要在Login文件夹下新建images文件夹

**配置路由跳转**

- 将路由组件映射为路由，在router下的index.js文件里进行配置
- 登录组件的路由是从个人中心Profile组件里跳转而来的，所以要修改Profile.vue

```html
<!--将class为profile-link的a标签替换为router-link-->
<a href="javascript:" class="profile-link">
    ...
</a>
<!--但要注意不要忘记class类名-->
<router-link to="/Login" class="profile-link">
    <!--先不考虑未登录的情况-->
    ...
</router-link>
```

**编写Login.vue代码**

- 利用`@click="$router.back()"`实现点击页面的箭头返回上一级路由/Profile的功能

**实现控制Footer的显示隐藏**

- 已确定底部的四个路由组件需要显示Footer部分

- 而Login组件为一级路由组件，且不需要显示底部的FooterGuide导航组件

- 所以为路由组件添加meta元数据来标识是否显示Footer，若路由没有showFooter属性，则判断时返回```false```

  ```javascript
  {
    path: '/msite',
    component: Msite,
    meta: {
      showFooter: true
    }
  },
  /*Order、Searh、Profile组件都要添加meta*/
  ```

- 在App.vue组件中通过代表当前路由的`$route`就能得到添加的meta属性，然后根据属性值来确定是否显示FooterGuide组件

  ```html
  <FooterGuide v-show="$route.meta.showFooter"></FooterGuide>
  ```

**其他细节**

- 注意到一个问题，在一个路由组件(Msite)将页面下拉，再切换到其他路由组件(Profile)，页面不会自动回到顶部。

  ```scss
  /*解决方法 其他页面中类似*/
  .profile
      width 100%
  	/*添加一行overflow hidden*/
      overflow hidden
  ```

## 9. 后台应用

### 9.1 说明

1. 整个项目为前后端分离的项目：mintshop-client 与 mintshop-server
2. 后台应用负责处理前台应用提交的请求, 并给前台应用返回 json 数据 
3. 前台应用负责展现数据, 与用户交互, 与后台应用交互 

### 9.2 运行

1. 确保启动 mongodb 服务 
2. 进入gshop-server_final文件夹，通过cmd端口启动服务器应用:  `npm start`，开启后端口号为 4000

### 9.3 API文档

**具体API文档详见gshop-server_final/API.md，然后可以使用Postman来进行接口测试**

## 10. 前后台交互 ajax 

- 测试完后台接口，则需要写前后台交互的ajax文件 
- 在src/api文件夹下新建index.js与ajax.js
  - 首先需要安装axios `npm install axios--save`

### 10.1 封装ajax请求函数

- 为了实现统一向后端发送请求数据，所以需要封装一个ajax请求函数

```javascript
/*
ajax 请求函数模块
*/ 
import axios from 'axios'
/**
 * 向外部暴漏一个函数 ajax
 * @param {*} url 请求路径，默认为空
 * @param {*} data 请求参数，默认为空对象
 * @param {*} type 请求方法，默认为GET
 */
export default function ajax(url = '', data = {}, type = 'GET') {
  // 返回值 Promise对象 （异步返回的数据是response.data，而不是response）
  return new Promise(function (resolve, reject) {
    //（利用axios）异步执行ajax请求
    let promise // 这个内部的promise用来保存axios的返回值(promise对象)
    if (type === 'GET') {
      // 准备 url query 参数数据
      let dataStr = '' // 数据拼接字符串，将data连接到url
      Object.keys(data).forEach(key => {
        dataStr += key + '=' + data[key] + '&'
      })
      if (dataStr !== '') {
        dataStr = dataStr.substring(0, dataStr.lastIndexOf('&'))
        url = url + '?' + dataStr
      }
      // 发送 get 请求
      promise = axios.get(url)
    } else {
      // 发送 post 请求
      promise = axios.post(url, data)
    }
    promise.then(response => {
        // 成功回调resolve()
        resolve(response.data)
      })
      .catch(error => {
        // 失败回调reject()
        reject(error)
      })
  })
}
```

- 通过对axios返回的promise对象再包装一层Promise的方法，提取返回数据中想要的部分，来简化外部的调用

### 10.2 封装接口请求函数

- 有了发送请求数据的ajax函数，还需要封装一些与后台交互的接口函数
- 根据接口文档来定义接口请求函数

```javascript
/*与后台交互模块 （依赖已封装的ajax函数）
 */
import ajax from './ajax'
/**
 * 获取地址信息(根据经纬度串)
 * 这个接口的经纬度参数是在url路径里的，没有query参数
 */
export const reqAddress = geohash => ajax(`/position/${geohash}`)
/**
 * 获取 msite 页面食品分类列表
 */
export const reqCategorys = () => ajax('/index_category')
/**
 * 获取 msite 商铺列表(根据query参数：经纬度)
 * 将经纬度两个数据作为一个参数对象传入
 * 也可以两个数据分别传入ajax， 然后再放入一个对象参数内， 如下面的手机号验证码接口
 */
export const reqShops = ({
  latitude,
  longitude
}) => ajax('/shops', {
  latitude,
  longitude
})
/**
 * 账号密码登录
 */
export const reqPwdLogin = (name, pwd, captcha) => ajax('/login_pwd', {
  name,
  pwd,
  captcha
}, 'POST')
/**
 * 获取短信验证码
 */
export const reqSendCode = phone => ajax('/sendcode', {
  phone
})
/**
 * 手机号验证码登录
 */
export const reqSmsLogin = (phone, code) => ajax('/login_sms', {
  phone,
  code
}, 'POST')
/**
 * 获取用户信息(根据会话)
 */
export const reqUser = () => ajax('/userinfo')
/*
 * 请求登出
 */
export const reqLogout = () => ajax('/logout')
```

### 10.3 配置代理并测试接口实现ajax跨域请求

**问题分析：**

- 目前为止运行的所有页面都是静态页面

- 接下来先测试使用封装的ajax接口请求函数来异步获取数据

  ```javascript
  // 先在App.vue中引入封装的接口函数
  import {reqCategorys} from './api'
  // 然后再调用接口，测试打印数据
  export default {
    async mounted () {
      const result = await reqCategorys()
      console.log(result)
    },
    components: {
      FooterGuide
    }
  }
  ```

- 打开浏览器，运行项目会报错` GET http://local:4000/index_category 404(Not Found)`

- 此时想起后端API端口为4000，然后在api文件夹下的index.js中修改测试

  ```javascript
  // 定义BASE_URL
  const BASE_URL = 'http://local:4000'
  // 然后修改请求接口的url
  export const reqCategorys = () => ajax(BASE_URL + '/index_category')
  ```

- 然后再打开项目，发现依然报错`access-control-allow-origin`提示请求为跨域请求

**配置代理并测试接口：**

- 除了设置让服务器允许跨域的方法之外，还可以通过配置代理实现跨域请求

1. 在项目config文件夹下的index.js文件里设置代理配置表 

  ```javascript
  // Paths
  // 静态资源文件夹
  assetsSubDirectory: 'static',
  // 发布路径
  assetsPublicPath: '/',
  
  // 代理配置表，在这里可以配置特定的请求代理到对应的API接口
  // 例如将'localhost:8080/api/xxx'代理到'www.example.com/api/xxx'
  proxyTable: {
    '/api': { // 匹配所有以 '/api'开头的请求路径
    	target: 'http://localhost:4000', // 代理目标的基础路径
      // secure: false,  // 如果是https接口，需要配置这个参数
      changeOrigin: true, // 支持跨域
      pathRewrite: { // 重写路径: 去掉路径中开头的'/api'
        '^/api': ''
      }
    }
  },
  ```

2. 修改api文件夹index.js里接口函数的请求路径

   ```javascript
   // const BASE_URL = 'http://local:4000'
   const BASE_URL = '/api'
   
   export const reqAddress = geohash => ajax(`${BASE_URL}/position/${geohash}`)
   export const reqCategorys = () => ajax(BASE_URL + '/index_category')
   // 下面修改后的接口省略...
   ```

3. 因为修改了项目的config文件，所以需要重启项目`npm run dev`

4. 此时可以在控制台看到跨域请求到的数据`{code: 0, data: Array(16)}`

## 11. 使用Vuex管理状态

- 安装Vuex`npm install vuex --save`用来管理从后台获取的状态数据
- 以下代码以首页Msite为例

### 11.1 创建Store(核心仓库)

- 在项目的store文件夹下新建index.js

```javascript
/*
vuex最核心的管理对象store
 */
// 首先引入Vue及Vuex
import Vue from 'vue'
import Vuex from 'vuex'

// 引入四个基本模块
import state from './state'
import mutations from './mutations'
import actions from './actions'
import getters from './getters'

// 一定要声明使用插件
Vue.use(Vuex)

// 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
export default new Vuex.Store({
  state,
  mutations,
  actions,
  getters
})
```

### 11.2 模块对象

#### 11.2.1 State

- 分析整理出项目首页Msite中有哪些状态需要管理，然后写入state.js

```javascript
/*
状态对象 state
 */
export default {
  latitude: 40.10038, // 纬度
  longitude: 116.36867, // 经度
  address: {}, // 地址相关信息对象
  categorys: [], // 食品分类数组
  shops: [] // 商家数组
}
```

#### 11.2.2 Mutation

- 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation 

- 每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)** 

- 我们可以使用常量替代 Mutation 事件类型，新建**mutations-types文件**

  ```javascript
  /*
  包含n个mutation的type名称常量
   */
  export const RECEIVE_ADDRESS = 'receive_address' // 接收地址信息
  export const RECEIVE_CATEGORYS = 'receive_categorys' // 接收分类数组
  export const RECEIVE_SHOPS = 'receive_shops' // 接收商家数组
  ```

- 然后在mutations.js文件内引入使用**(注意书写格式)**

```javascript
/*
vuex 的 mutations 模块
*/
import {RECEIVE_ADDRESS,RECEIVE_CATEGORYS,RECEIVE_SHOPS} from './mutation-types'

// [方法名](state,{param}){}
export default {
	[RECEIVE_ADDRESS](state, {address}) {
		state.address = address
	},
	[RECEIVE_CATEGORYS](state, {categorys}) {
		state.categorys = categorys
	},
	[RECEIVE_SHOPS](state, {shops}) {
		state.shops = shops
	}
}
```

- 而回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数 

#### 11.2.3 Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

 ```javascript
// Action:通过操作mutation间接更新state的多个方法的对象
// 注意要引入api接口函数
import {reqAddress, reqCategorys, reqShops} from '../api'
import {RECEIVE_ADDRESS, RECEIVE_CATEGORYS, RECEIVE_SHOPS} from './mutation-types'
export default {
  // 异步获取地址
  async getAddress ({commit, state}) {
    // 从state状态中获取到经纬度用来设置reqAddress的参数（看接口文档）
    const geohash = state.latitude + ',' + state.longitude
    
    // 1. 发送异步ajax请求
    const result = await reqAddress(geohash)
    // 2. 根据结果提交一个mutation
    commit(RECEIVE_ADDRESS, {address: result.data})
  },
  // 异步获取分类列表
  async getCategorys ({commit}) {
    const result = await reqCategorys()
    commit(RECEIVE_CATEGORYS, {categorys: result.data})
  },
  // 异步获取商家列表
  async getShops ({commit, state}) {
    // 对象的解构赋值
    const {latitude, longitude} = state
    // 注意参数的顺序
    const result = await reqShops({latitude, longitude})
    commit(RECEIVE_SHOPS, {shops: result.data})
  }
}
 ```
- 至此已经完成了首页状态数据的vuex设置

### 11.3 异步获取并显示数据

1. 在项目中注册store

   ```javascript
   //项目的main.js文件
   import store from './store'
   
   new Vue({
   	store
   })
   ```

2. 测试异步**获取**当前地址数据

   ```javascript
   // 地址信息要尽早的获取，所以请求可以写在App.vue中
   // 首先删除之前测试使用封装的ajax接口的代码
   async mounted () {
       // 通过this.$store.dispatch 方法触发调用Action
       this.$store.dispatch('getAddress')
   }
   ```

   - 运行项目，在浏览器控制台里打开vue开发工具，切换到vuex，可以看到通过vuex异步获取的状态数据
   - 除了这种方法调用action，还可以使用mapActions语法糖

   ```javascript
   import {mapActions} from 'vuex'
   
   async mounted () {
       this.getAddress()
   }
   methods: {
   	...mapActions(['getAddress'])
   }
   ```

3. **读取**并显示获取到的当前地址数据

   - 首页Msite组件中显示地址信息

   ```javascript
   // 利用mapState语法糖去读取state对象
   import {mapState} from 'vuex'
   
   computed: {
   	...mapState(['address'])
   }
   ```

   ```javascript
   <HeaderTop :title="address.name">
   // 将静态地址信息换成异步获取的地址数据 注意:title为绑定数据
   </HeaderTop>
   ```
## 12. 完成Msite页面
### 12.1 异步显示食品分类轮播列表

- 上面已经完成了首页当前地址信息的获取与显示，接下来是完成Msite.vue中的食品分类的轮播图

1. mounted方法中通过this.$store.dispatch调用Action来获取异步数据

   ```javascript
   // 忘记Actuon里对应的方法名时查看Action.js
   mounted () {
       this.$store.dispatch('getCategorys')
       ...
   }
   ```

2. 通过mapState语法糖来读取获取到的异步数据

   ```javascript
   // 忘记state名时查看State.js
   computed: {
       ...mapState(['address', 'categorys'])
   }
   ```

3. 分析template结构并处理categorys数据

   > 业务分析： 此时获取到的categorys是所有食品类别的一维数组，而如果要完成轮播图，需要将其变成这种`[[page1], [page2], [page3]]`二维数组categorysArr。
   >
   > 比如本项目中有两页轮播图，一页为8个数据。应该把categorys数组处理成`[[data1 - 8], [data1 - 8]]`这种数据格式

   ```html
   //template
   <div class="swiper-slide" v-for="(pages,index) in categorysArr" :key="index">
   	<a href="javascript:" class="(data,index) in pages" :key="index">
           ......
       </a>
   </div>
   ```

   - 利用计算属性将categorys处理成期望的数据结构categorysArr

   ```javascript
   computed () {
       ...mapState(['address', 'categorys']),
       /*
       根据categorys一维数组生成一个2维数组
       小数组中的元素个数最大是8
       */
       categorysArr () {
         // 1.先从当前组件中得到所有食品分类的一维数组
         const {categorys} = this
         // 2.准备一个空的二维数组--categorysArr
         const arr = []
         // 3.准备一个小数组--pages(最大长度为8)
         let minArr = []
         // 4.遍历categorys得到处理后的二维数组catagorysArr
         categorys.forEach(data => {
           // 如果当前小数组(pages)已经满了, 创建一个新的
           if (minArr.length === 8) {
             minArr = []
           }
           // 如果minArr是空的, 将小数组(pages)保存到大数组(categorysArr)中
           if (minArr.length === 0) {
             arr.push(minArr)
           }
           // 将当前分类信息保存到小数组(pages)中
           minArr.push(data)
         })
         return arr
       }
   }
   ```

   > 注意第4步forEach里的逻辑顺序
   >
   > 1.为什么当minArr数组的长度为0时就将它与大数组关联起来，不是等它存满(8个)？
   >
   > 因为有可能categorys里的数据个数不为8的倍数，最后一个minArr内的数据不足8个。
   >
   > 2.为什么先判断minArr的长度为8的情况再判断等于0的情况？
   >
   > 因为先判断minArr的长度为0，将其放入大数组中与大数组关联起来，那么等到当前这个minArr填充完成之后新建的小数组则无法与大数组关联起来。
   >
   > 所以先判断长度为8的情况，再判断数组长度为0的情况。就可以确保之前的minArr填充完成后，新建的minArr都可以被放到大数组里与大数组关联起来。

   - 除了上面的方式处理categorys，也可以采用分割数组的方法。

   ```javascript
   categorysArr () {
   	const {categorys} = this
   	const arr = []
    	for (let i = 0,len = categorys.length;i < len; i += 8){
       	arr.push(categorys.slice(i, i + 8))
   	}
       return arr
   }
   ```

4. 将数据显示到页面上

   ```javascript
   // 因为食品分类的图片信息都有一个baseImageUrl所以在data里定义
   data () {
       return {
         baseImageUrl: 'https://fuss10.elemecdn.com'
   	}
   },
   ```

   ```html
   <!--通过v-for循环渲染data的信息 注意src为绑定数据--> 
   <div class="swiper-slide" v-for="(pages,index) in categorysArr" :key="index">
   	<a href="javascript:" class="link_to_food" v-for="(data,index) in pages" :key="index">
   		<div class="food_container">
   			<img :src="baseImageUrl+data.image_url">
   		</div>
   		<span>{{data.title}}</span>
   	</a>
   </div>
   ```

### 12.2 使用watch与$nextTick解决轮播的Bug

- 分页器Swiper其实应该是在轮播列表显示(即categorys数组有了数据)以后才初始化。

- 最开始categorys为空数组，有了数据才会显示轮播列表，而要监视categorys的数据变化，就要用到**watch**。

  ```javascript
  // 新建watch 监听categorys
  watch: {
      categorys (value) { // categorys数组中有数据了
      	// 但界面还没有异步更新
      }
  }
  // 删除mounted中的new Swiper...代码
  ```

- 但其实**state里的状态数据改变（categorys接收数据）与异步更新界面（显示轮播列表）是两个步骤**。所以需要等一等，界面完成异步更新后才可以进行Swiper的初始化。

  ```javascript
  // 使用setTimeout可以实现效果, 但是时机不准确
  setTimeout(() => {
  	// 创建一个Swiper实例对象, 来实现轮播
  	new Swiper('.swiper-container', {
            autoplay: true,
            // 如果需要分页器
            pagination: {
              el: '.swiper-pagination',
              clickable: true
            }
  	})
  }, 100)
  ```

- 利用`vm.$nextTick( [callback] )`来实现等待界面完成异步更新就立即创建Swiper对象 

  ```javascript
  // 在修改数据之后立即使用它，然后等待 DOM 更新。
  this.$nextTick(() => {
  	// 一旦完成界面更新, 立即执行回调
      new Swiper('.swiper-container', {
      	autoplay: true,
      	pagination: {
      	el: '.swiper-pagination',
      	clickable: true
      }
  })
  ```

### 12.3 异步显示商家列表

1. 在Msite的mounted中获取异步数据

```javascript
this.$store.dispatch('getShops')
```

2. 在ShopList.vue中引入mapState，在computed中读取状态数据

```javascript
import {mapState} from 'vuex'
export default {
  	computed: {
		...mapState(['shops'])
	}
}
```

3. 修改template并添加data

```javascript
data () {
	return {
      baseImgUrl: 'http://owoccema2.bkt.clouddn.com/show/MintShop/'
    }
}
```

```html
<li class="shop_li border-1px" v-for="(shop,index) in shops" :key="index">
	<a>
		<div class="shop_left">
			<img class="shop_img" :src="baseImgUrl + shop.image_path">
     	</div>
     	<div class="shop_right">
     		<section class="shop_detail_header">
             	<h4 class="shop_title ellipsis">{{shop.name}}</h4>
             	<ul class="shop_detail_ul">
                	...
              	</ul>
         	</section>
         	<section class="shop_rating_order">
         	<section class="shop_rating_order_left">
        	 	...
         	</section>
		</div>
	</a>
</li>
```

### 12.4 开发评分Star组件

- 不仅在首页的商家列表中有五颗星星的评分部分，商家信息的评价区域也有star评分，所以可以把它抽取成一个可复用的一般组件。

1. 在components文件夹下新建Star文件夹，然后将原本在ShopList文件夹下的images文件夹里的stars移动到新建的Star文件夹里并重命名为images

2. 在Star文件夹里新建Star.vue并将ShopList.vue中评分部分的模版和样式剪切进去，注意修改图片路径

   ```html
   <div class="star star-24">
   	<span class="star-item on"></span>
   	<span class="star-item on"></span>
   	<span class="star-item on"></span>
   	<span class="star-item half"></span>
   	<span class="star-item off"></span>
   </div>
   ```

3. 根据类名为组件设置属性props和计算属性

   ```javascript
   // 类名常量
   const CLASS_ON = 'on'
   const CLASS_HALF = 'half'
   const CLASS_OFF = 'off'
   export default {
     props: {
       score: Number,
       size: Number
     },
     computed: {
       /*
       3.2: 3 + 0 + 2
       4.7: 4 + 1 + 0
       */
       // 该方法产生一个数组starArr来表示on half off类名的span数量（总长度为5）
       starClasses () {
         const {score} = this
         const starArr = []
         // 向starArr中添加n个CLASS_ON
         const scoreInteger = Math.floor(score)
         for (let i = 0; i < scoreInteger; i++) {
           starArr.push(CLASS_ON)
         }
         // 向starArr中添加0/1个CLASS_HALF
         if(score*10-scoreInteger*10>=5) {
           starArr.push(CLASS_HALF)
         }
         // 向starArr中添加n个CLASS_OFF
         while(starArr.length<5) {
           starArr.push(CLASS_OFF)
         }
         return starArr
       }
     }
   }
   ```

4. 修改template模版

   ```html
   <div class="star" :class="'star-'+size">
   	<span class="star-item" v-for="(sc, index) in starClasses" :class="sc" :key="index"></span>
   </div>
   ```

5. 在ShopList中import引入并注册使用

   ```html
   <Star :score="shop.rating" :size="24"></Star>
   ```

###  12.5 使用svg显示页面加载中提示界面

- 目前首页的内容数据已经可以异步获取并显示，但在数据还未加载完成时，为了优化用户体验，应该给出页面加载中的提示界面。

- 首先将svg资源图片放入相应的Msite和ShopList的images文件夹里

- 然后修改模版的显示条件

  ```html
  <!--Msite-->
  <div class="swiper-container" v-if="categorys.length">
  	...
  </div>
  <img src="./images/msite_back.svg" alt="back" v-else>
  ```

  ```html
  <!--ShopList-->
  <ul class="shop_list" v-if="shops.length">
      ...
  </ul>
  <!-- 循环显示6个svg图片 -->
  <ul v-else>
  	<li v-for="item in 6" :key="item">
  		<img src="./images/shop_back.svg" alt="back">
  	</li>
  </ul>
  ```

## 13. 实现登陆注册功能 

### 13.1 模块界面效果及交互功能说明

**1. 界面相关效果**

1. 切换登陆方式 
2. 手机号合法检查 
3. 倒计时效果 
4. 切换显示或隐藏密码 
5. 前台验证提示 

**2. 前后台交互功能 **

1. 动态一次性图形验证码 
2. 动态一次性短信验证码 
3. 短信登陆 
4. 密码登陆 
5. 获取用户信息,实现自动登陆 
6. 退出登陆

### 13.2 效果一：切换登陆方式

1. 既然要切换登陆方式，就需要一个变量来标识这两种方式

```javascript
data () {
	return {
		loginWay: false // true代表短信登陆, false代表密码
	}
}
```

2. 动态绑定a标签的class和点击事件来实现点击切换样式

```html
 <div class="login_header_title">
	<a href="javascript:;" :class="{on: loginWay}" @click="loginWay=true">短信登录</a>
	<a href="javascript:;" :class="{on: !loginWay}" @click="loginWay=false">密码登录</a>
</div>
```

3. 同时实现下面的表单登陆框自动切换

```html
<div class="login_content">
    <form>
        <div :class="{on: loginWay}">
            短信登陆的input...
        </div>
        <div :class="{on: !loginWay}">
            密码登陆的input...
        </div>
    </form>
</div>
```

### 13.3 效果二：手机号合法检查

- 既然是要对手机号格式进行检查就要为其绑定数据

  ```html
  <!--v-model的值最好按照api文档里要求的字段来命名-->
  <input type="tel" maxlength="11" placeholder="手机号" v-model="phone">
  ```

- 根据手机号格式是否正确来动态的为获取验证码添加一个类名right_phone

  ```html
  <button disabled="disabled" class="get_verification" :class="{right_phone:rightPhone}">获取验证码</button>
  ```

- right_phone的值是根据phone来确定的，所以应该是一个计算属性

  ```javascript
  computed: {
    rightPhone () {
      // 利用正则对手机号进行匹配，返回布尔值
      return /^1\d{10}$/.test(this.phone)
    }
  }
  ```

- 此时button的disabled也应该根据rightPhone的返回值来确定

  ```html
  <button :disabled="!rightPhone" class="get_verification" :class="{right_phone:rightPhone}">获取验证码</button>
  ```

- 在style部分加入新定义的right_phone样式

  ```scss
  .get_verification
  	...
  	&.right_phone
      	color black
  ```
  
### 13.4 效果三：30s倒计时

- 点击获取验证码之后会显示30s倒计时的效果并发送获取验证码的请求

- 注意要阻止点击button的默认提交表单事件，所以用`@click.prevent`

  ```html
  <button :disabled="!rightPhone" class="get_verification" :class="{right_phone:rightPhone}" @click.prevent="getCode">获取验证码</button>
  ```

- 在data里定义定时变量computeTime，然后感觉时间判断显示的内容

  ```html
  <button class="get_verification">{{computeTime>0 ? `(${computeTime}s)已发送` : '获取验证码'}}</button>
  ```

- 然后在methods里定义定时器

  ```javascript
  methods: {
      getCode () {
       // 如果当前没有计时!this.computeTime等于this.computeTime === 0
         if(!this.computeTime) {
           // 启动倒计时
           this.computeTime = 30
           this.intervalId = setInterval(() => {
             this.computeTime--
             if(this.computeTime <= 0) {
               // 停止计时
               clearInterval(this.intervalId)
             }
           }, 1000)
         }
       // 发送ajax请求（向指定手机号发送验证码短信）
     }
   }
  ```


### 13.5 效果四： 密码的显示隐藏

- 利用两个type不同的input来实现密码的显示隐藏

- 在data里定义showPwd(默认为false)控制两者的显隐，同时使用v-model绑定数据pwd

  ```html
  <section class="login_verification">
  	<input type="text" maxlength="8" placeholder="密码" v-if="showPwd" v-model="pwd">
  	<input type="password" maxlength="8" placeholder="密码" v-else v-model="pwd">
      ...
  </section>
  ```

- 为滑块区域添加点击监听，用来切换showPwd的值

- 同时切换滑块的各种样式

  ```html
  <div class="switch_button" :class="showPwd?'on':'off'" @click="showPwd=!showPwd">
  	<div class="switch_circle" :class="{right: showPwd}"></div>
  	<span class="switch_text">{{showPwd ? 'abc' : '...'}}</span>
  </div>
  ```

- 添加必要的style

  ```scss
  >.switch_circle
  	...
  	&.right
      	transform translateX(30px)
  ```

### 13.6 效果五：前台表单验证

1. 首先阻止点击登录button的默认提交表单事件

   ```html
   <form @submit.prevent = "login">
       ...
   </form>
   ```

2. 在methods里定义login方法并收集表单数据(即为未使用v-model绑定data的input添加data)

   ```javascript
   data () {
       return {
           ...
           name: '', // 用户名
           code: '', // 短信验证码
           captcha: '', // 图形验证码
       }
   }
   ```

3. 在login里根据不同的登录方式对收集的数据进行检查

   ```javascript
   if(this.loginWay) {  // 短信登陆
   	if(!this.rightPhone) {
   		// 手机号不正确
       	return
   	} else if(!/^\d{6}$/.test(code)) {
           // 验证必须是6位数字
           return
       }
   }else {	// 密码登陆
       ...
   }
   ```

4. 要将验证失败的提示信息显示出来，可以使用自定义AlertTip弹窗组件(开发中可以用第三方插件)

   ```javascript
   import AlertTip from '../../components/AlertTip/AlertTip.vue'
   ...
   export default {
   	...
   	components: {
       	AlertTip
   	}
   }
   ```

5. 为AlertTip定义状态数据

   ```javascript
   data () {
       return {
           ...
           alertText: '', // 提示文本
           alertShow: false, // 是否显示警告框
       }
   }
   ```

6. 在页面中放置弹窗组件

   ```html
   <section>
   	...
       <AlertTip :alertText="alertText" v-show="alertShow" @closeTip="closeTip"/>
   </section>
   ```

7. 抽取显示弹窗和关闭弹窗的方法，并在login方法和AlertTip中使用

   ```javascript
   showAlert (alertText) {
   	this.alertShow = true
   	this.alertText = alertText
   },
   closeTip () {
   	this.alertShow = false
   	this.alertText = ''
   },
   login () {
   	if(this.loginWay) {  // 短信登陆
   		if(!this.rightPhone) {
   			// 手机号不正确
               this.showAlert('手机号不正确')
       		return
   		} else if(!/^\d{6}$/.test(code)) {
           	this.showAlert('验证必须是6位数字')
           	return
       	}
   	}...
   }
   ```

### 13.7 功能一：图形验证码

- 根据API文档，动态一次性图形验证码的接口为`http://localhost:4000/captcha`

- 同时为这个img添加点击事件，让其可以点击重新发送请求，刷新图片

  ```html
  <img class="get_verification" src="http://localhost:4000/captcha" alt="captcha" @click="getCaptcha" ref="captcha">
  ```

- 为这个img元素添加ref属性，方便在getCaptcha中使用

- 利用时间戳使其每次加载时的请求参数不一样

  ```javascript
  getCaptcha () {
  	// 每次指定的src要不一样
      this.$refs.captcha.src = 'http://localhost:4000/captcha?time='+Date.now()
  }
  ```

### 13.8 功能二：短信验证码

- 利用第三方短信验证码平台(容联云通讯)提供的接口来实现

- 先在服务端项目将自己的sid与token填入util文件夹下的sms_util.js文件

- 再在客户端的登录组件中引入接口请求函数(这是为了方便处理，同样也可以在action里调用)

  ```javascript
  import {reqSendCode, reqSmsLogin, reqPwdLogin} from '../../api'
  ```

- 在getCode方法里进行调用

  ```javascript
  // 请求返回的是promise对象，所以用到了async await
  async getCode () {
      ...
      // 发送ajax请求(向指定手机号发送验证码短信) 
      const result = await reqSendCode(this.phone)
      if(result.code===1) { // 手机号验证失败
      	// 显示提示
          this.showAlert(result.msg)
          // 停止计时
          if(this.computeTime) {
          	this.computeTime = 0
              clearInterval(this.intervalId)
              this.intervalId = undefined
          }
  	}
  }
  // 只要手机号填写正确 短信验证码也可以在服务端的控制台中查看
  ```

### 13.9 功能三：短信与密码登录

- 在login方法里完成短信和密码登录的aiax请求

  ```javascript
  async login () {
      let result // 保存登录成功后返回的数据
      	...
      	// 发送ajax请求短信登陆
      	result = await reqSmsLogin(phone, code)
      	...
      	// 发送ajax请求密码登陆
      	result = await reqPwdLogin({name, pwd, captcha})
      ...
  }
  ```
- 点击登录发送请求的同时停止计时器，然后将请求的结果进行处理
  ```javascript
   ...
  // 停止计时
  if(this.computeTime) {
  	this.computeTime = 0
      clearInterval(this.intervalId)
      this.intervalId = undefined
  }
  // 根据结果数据处理
  if(result.code===0) { // 成功
  	const user = result.data
   	// 将user信息保存到vuex的state
      // todo
      // 去个人中心界面
      this.$router.replace('/profile')
  } else {
      // 显示新的图片验证码
      this.getCaptcha()
      // 显示警告提示
      const msg = result.msg
      this.showAlert(msg)
  }
  ```
- 测试用的用户名: abc，密码: 123。

### 13.10 功能四：获取用户信息,实现自动登陆

**1. 将用户信息保存到vuex**

1. 在state中添加用户信息的状态数据userInfo

   ```javascript
   userInfo: {} // 用户信息
   ```

2. 在mutation-types中定义常量

   ```javascript
   export const RECEIVE_USER_INFO = 'receive_user_info' // 接收用户信息
   ```

3. 在mutations文件中增加改变state的方法

    ```javascript
       //先import引入RECEIVE_USER_INFO
       [RECEIVE_USER_INFO] (state, {userInfo}) {
       state.userInfo = userInfo
     }
    ```

4. 在actions文件中增加同步用户信息的方法

   - 因为之前已经异步获取了用户信息，所以这里将用户信息state更新的操作是同步的

   ```javascript
   // 要先引入RECEIVE_USER_INFO这个mutation
   // 同步记录用户信息 
     recordUser ({commit}, userInfo) {
       commit(RECEIVE_USER_INFO, {userInfo})
     }
   ```

5. 在Login组件中调用这个action

   ```javascript
   // 将user保存到vuex的state
   this.$store.dispatch('recordUser',user)
   ```

6. 之后可以在个人中心Profile页面读取并显示用户信息userInfo

   ```javascript
   import {mapState} from 'vuex'
   ...
   computed: {
       ...mapState(['userInfo'])
   }
   ```

   ```html
   <p class="user-info-top">{{userInfo.name || '登录/注册'}}</p>
   ```


**2. 更新登录后的个人中心界面**

- 使用用户名和密码登录时显示用户名和绑定的手机号信息

- 使用手机号登录时只需要显示手机号

- 根据用户是否登录来定义a标签不同的路由

  ```html
  <!--userinfo路由对应的页面暂时未做-->
  <router-link :to="userInfo._id ? '/userinfo': '/login'">
      ...
  	<div class="user-info">
   		<p class="user-info-top" v-if="!userInfo.phone" >			{{userInfo.name || '登录/注册'}}</p>
  		<p>
      	...
  		<span class="icon-mobile-number">{{userInfo.phone || '暂无绑定手机号'}}</span>
  		</p>
  	</div>
  </router-link>	
  ```

- 同时要注意首页Msite顶部的信息也要进行更改

  ```html
  <router-link class="header_login" slot="right" :to="userInfo._id ? '/userinfo': '/login'">
   	<span class="header_login_text" v-if="!userInfo._id">
            登录|注册
  	</span>
      <!--如果已经登录则显示一个icon-->
  	<span class="header_login_text" v-else>
  		<i class="iconfont icon-yonghuming"></i>
  	</span>
  </router-link>
  ```

**3. 完成自动登录功能**

- 服务器端的routes文件夹下的index.js中已经定义了返回用户信息的方法

  ```javascript
  // 其中她将用户的userid取出来放入一个session会话中
  router.get('/userinfo', function (req, res) {
    // 取出userid
    const userid = req.session.userid
    // 查询
    UserModel.findOne({_id: userid}, _filter, function (err, user) {
      // 如果没有, 返回错误提示
      if (!user) {
        // 清除浏览器保存的userid的cookie
        delete req.session.userid
  
        res.send({code: 1, msg: '请先登陆'})
      } else {
        // 如果有, 返回user
        res.send({code: 0, data: user})
      }
    })
  })
  ```

- app.js中已经定义了这个用户登录的session会话的维持时间为24h

  ```javascript
  app.use(session({
    secret: '12345',
    cookie: {maxAge: 1000*60*60*24 },  //设置maxAge是80000ms，即80s后session和相应的cookie失效过期
    resave: false,
    saveUninitialized: true,
  }));
  ```

- api中的对应接口已经完成

  ```javascript
  // 获取用户信息(根据会话)
  export const reqUserInfo = () => ajax(BASE_URL + '/userinfo')
  ```

- 在action中定义一个方法来调用这个接口

  ```javascript
  // 异步获取用户信息(先引入reqUserInfo接口)
  async getUserInfo ({commit}) {
      const result = await reqUserInfo()
      if (result.code === 0) {
        const userInfo = result.data
        commit(RECEIVE_USER_INFO, {userInfo})
      }
    }
  ```

- 最后在App.vue中引入action并触发

  ```javascript
  async mounted () {
  	...
      // this.getAddress()
      this.getUserInfo()
    },
  methods: {
      ...mapActions(['getUserInfo'])
    }
  ```

### 13.11 功能五：退出登录

- 用户登录后在个人中心页面添加一个退出登录的按钮

- 下载安装mint-ui来实现

  ```bash
  // 安装mint-ui
  npm install --save mint-ui
  ```

- 实现自动按需打包

  ```bash
  // 安装工具包
  npm install --save-dev babel-pulgin-component
  ```

  ```javascript
  // 配置
  "plugins": ["transform-runtime",["component", [
  	{
  		"libraryName": "mint-ui",
  		"style": true
  	}
  ]]]
  ```

- 引入并注册使用mint-ui的标签组件

  ```javascript
  // 在入口的main.js引入Button
  import {Button} from 'mint-ui'
  
  // 注册全局组件
  Vue.component(Button.name, Button)
  ```

- 在Profile页面中使用mint-ui的标签

  ```html
  <section class="profile_my_order border-1px">
      <!--只有用户登录之后才会显示 同时绑定点击事件-->
  	<mt-button type="danger" style="width: 100%" v-if="userInfo._id" @click="logout">退出登录</mt-button>
  </section>
  ```

- 引入mint-ui的confirm确认和toast文本提示框

  ```javascript
  import { MessageBox, Toast } from 'mint-ui'
  
  logout () {
  	MessageBox.confirm('确认退出吗?').then(
  		action => {
           // 请求退出
           this.$store.dispatch('logout')
           	Toast('登出完成')
           },
  		action => {
              console.log('取消登录')
  		}
  	)
  }
  ```

- 在actions.js中定义退出登录的方法

  ```javascript
  // 首先引入api的index.js中定义的reqLogout接口和mutation
  // 异步登出
  async logout ({commit}) {
  	const result = await reqLogout()
  	if (result.code === 0) { commit(RESET_USER_INFO)}
  }
  ```

  ```javascript
  //同时改动下列两个文件
  //mutations-types文件
  export const RESET_USER_INFO = 'receive_user_info' // 重置用户信息
  
  //mutations文件
  [RESET_USER_INFO] (state) {
      state.userInfo = {}
    }
  ```

## 14. 商家店铺界面

### 14.1 商家店铺的整体页面搭建

> 通过点击商家列表(ShopList)里的某一项进入商家店铺的一级路由界面(Shop.vue)，商家店铺界面包括顶部的一个头部的一般组件(ShopHeader.vue)和下面三个可以切换的路由子组件(ShopGoods、ShopInfo、ShopRatings)

1. 在router文件夹下的index.js文件中将路由组件注册成路由

  ```javascript
  {
  	path: '/shop',
  	component: Shop,
  	children: [{
  		path: '/shop/goods',
  		component: ShopGoods
  	},
  	{
          path: '/shop/ratings',
          component: ShopRatings
  	},
  	{
  		path: '/shop/info',
          component: ShopInfo
  	},
  	{
          path: '',
          redirect: '/shop/goods'
  	}]
  }
  ```

2. 在ShopList.vue中为商家列表添加点击事件

   ```html
   <ul class="shop_list" v-if="shops.length">
       <li class="shop_li border-1px" v-for="(shop,index) in shops" :key="index" @click="$router.push('/shop')">
       ...
       </li>
   </ul>
   ```

3. Shop.vue中引入各路由组件然后在模版中使用

   ```html
   <div>
   	<ShopHeader></ShopHeader>
   	<div class="tab">
   		<div class="tab-item">
            <!-- 使用replace的方式进行路由切换 （路由懒加载） -->
   			<router-link to="/shop/goods" replace>点餐				</router-link>
   		</div>
   		<div class="tab-item">
   			<router-link to="/shop/ratings" replace>评价				</router-link>
   		</div>
   		<div class="tab-item">
   			<router-link to="/shop/info" replace>商家					</router-link>
   		</div>
   	</div>
       <!-- 缓存路由组件对象 -->
   	<keep-alive>
   		<router-view/>
   	</keep-alive>
   </div>
   ```

### 14.2 Json模拟数据

**设计json数据的结构**

> 商家店铺界面包括点餐、评价、和商家信息三个部分的数据(头部显示的数据也是商家信息)，而且这三个方面的数据直接没有顺序关系，可以使用对象结构来存储它们。

```json
// 点餐数据里包括各种不同分类的食品，可以用数组goods[]来存放这些数据对象(没有顺序关系但属于同一类型)
// 每一类食品除了分类名称name还有一个foods数据来存放这一类的食品
// foods数组内的每个对象都是一个食品实例
{
    "goods":[
        {
            name: "精选套餐",
            foods: [
                {
                    name: "南瓜粥",
                    price: 9
                }
            ]
        }
    ],
    "ratings":[
        {}
    ],
    "info":｛｝
}
// 评价数据都属于同一类型，可以使用数组来存放[{评价一},{评价二}..]
// 商家信息数据没有顺序，可以统一保存到对象内
```

- 将设计好的data.json模拟数据保存在mock文件夹内

**使用mockjs模拟数据接口**

```bash
npm install --save mockjs
```

- 在mock文件夹下定义mockServer文件配置mock模拟数据的接口

```javascript
/*
使用mockjs提供mock数据接口
 */
import Mock from 'mockjs'
import data from './data.json'

// 返回goods的接口
Mock.mock('/goods', {code: 0, data: data.goods})
// 返回ratings的接口
Mock.mock('/ratings', {code: 0, data: data.ratings})
// 返回info的接口
Mock.mock('/info', {code: 0, data: data.info})

// export default ???  不需要向外暴露任何数据, 只需要保存能执行即可
```

- 在main.js中加载mockServer文件即可

  ```javascript
  import './mock/mockServer.js'
  ```

**ajax请求mockjs模拟的数据**

1. api/index.js中定义ajax请求方法

   ```javascript
   /*
    * 获取商家信息(下列请求由mock拦截并返回 不需要代理)
    */
   export const reqShopInfo = () => ajax('/info')
   /**
    * 获取商家评价数组
    */
   export const reqShopRatings = () => ajax('/ratings')
   /**
    * 获取商家商品数组
    */
   export const reqShopGoods = () => ajax('/goods')
   ```

2. 再写一套用来管理从后台接收到的数据vuex配置

   ```javascript
   // 1. state
   goods: [], // 商品列表
   ratings: [], // 商家评价列表
   info: {} // 商家信息
   
   // 2. mutations-type
   export const RECEIVE_GOODS = 'receive_goods' // 接收商品数组
   export const RECEIVE_RATINGS = 'receive_ratings' // 接收商家评价数组
   export const RECEIVE_INFO = 'receive_info' // 接收商家信息
   
   // 3. mutations
   [RECEIVE_INFO] (state, {info}) {
     state.info = info
   },
   
   [RECEIVE_RATINGS] (state, {ratings}) {
     state.ratings = ratings
   },
   
   [RECEIVE_GOODS] (state, {goods}) {
     state.goods = goods
   }
   
   // 4. action
   // 异步获取商家信息
     async getShopInfo ({commit}) {
       const result = await reqShopInfo()
       if (result.code === 0) {
         const info = result.data
         commit(RECEIVE_INFO, {info})
       }
     },
   
     // 异步获取商家评价列表
     async getShopRatings ({commit}) {
       const result = await reqShopRatings()
       if (result.code === 0) {
         const ratings = result.data
         commit(RECEIVE_RATINGS, {ratings})
       }
     },
   
     // 异步获取商家商品列表
     async getShopGoods ({commit}) {
       const result = await reqShopGoods()
       if (result.code === 0) {
         const goods = result.data
         commit(RECEIVE_GOODS, {goods})
       }
     }
   ```

3. 在shop.vue中测试获取商家信息数据

   ```javascript
   // 可以在控制台的vuex中查看到info数据
   mounted () {
   	this.$store.dispatch('getShopInfo')
   }
   ```

### 14.3 ShopHeader模板

- 已经可以获取到mock的模拟数据，接下来开始修改商家界面的头部模板

- 修改完模版和样式代码，然后读取vuex里的数据

  ```javascript
  import {mapState} from 'vuex'
  export default {
    computed: {
        ...mapState(['info'])
      }
  }
  ```

- 并将info里的数据渲染到模版中，其中有几点需要注意

  1. 为nav动态绑定背景style(删除原来的样式里的背景图片)

     ```css
     :style="{backgroundImage: `url(${info.bgImg})`}"
     ```

  2. 在style里定义三种不同颜色的优惠活动的类名并放进数组，然后把它们和info.supports.type关联起来

     ```javascript
     data () {
       return {
         // 注意顺序要与info的type对应
         supportClasses: ['activity-green', 'activity-red', 'activity-orange']
       }
     }
     ```

     ```html
      <div class="activity" :class="supportClasses[info.supports[0].type]">
      </div>
     ```

     这样写会报一个错误`Error in render: "TypeError: Cannot read property '0' of undefined"`因为vuex的数据是异步的，而页面刚加载时info为空对象，info.supports不存在，为undefined。所以再取它下标为0的值会报这个错误。(二级表达式info.bgImg并不会报错)

     使用v-if来避免没有数据时也会解析模版

     ```html
     <div class="shop-header-discounts" v-if="info.supports" @click="toggleSupportShow"></div>
     ```

  3. 利用shopShow和supportShow来标识模态框和优惠活动列表是否显示，同时定义切换显隐的方法

     ```javascript
     data () {
       return {
           ...
     	shopShow: false,
     	supportShow: false
       }
     }，
     methods: {
       toggleShopShow () {
         this.shopShow = !this.shopShow
       },
       toggleSupportShow () {
         this.supportShow = !this.supportShow
       }
     }
     ```

  4. 可以为弹窗添加一个transition动画(activity-sheet一样)

     ```html
     <transition name="fade">
         <div class="shop-brief-modal" v-show="shopShow"></div>
     </transition>
     ```

     然后找到shop-brief-modal的样式 添加动画过程

     ```css
     &.fade-enter-active, &.fade-leave-active {
     	transition: opacity 0.5s;
       }
     
     &.fade-enter, &.fade-leave-to {
     	opacity: 0;
       }
     ```


###  14.4 异步显示ShopGoods数据

> 完成了头部的ShopHeader，接下来是点餐部分的ShopGoods组件。此组件是一个比较复杂的路由组件，主要包含了3个部分：ShopCart组件（底部的购物车）、CartControl组件（购物车里的加减商品按钮组件）、Food组件（点击商品图片查看详细信息的弹窗）
>
> 另外还使用了第三方库 better-scroll: 处理UI 滑动 

- 左右结构的模版布局

  ```html
  <div class="goods">
      <!-- 左侧的菜单选项-->
      <div class="menu-wrapper" ref="menuWrapper">
      	<!-- 菜单对应的是食物分类列表-->
          <ul>
              <li>折扣</li>
              <li>优惠</li>
              <li>爽口凉菜</li>
              <li>...</li>
          </ul>
      </div>
      <!-- 右侧的食物列表-->
      <div class="foods-wrapper" ref="foodsWrapper">
      	<!-- 右侧的食物列表是根据左侧的分类列表展现的-->
          <!-- 所以右侧是在一个分类标题列表里面嵌套着各类食物列表-->
          <ul>
            <li class="food-list-hook">
              <h1 class="title">折扣</h1>
              <ul>
             	  <li>南瓜粥</li>
                <li>红豆薏米美肤粥</li>
              </ul>
            </li>
            <li class="food-list-hook">
              <h1 class="title">优惠</h1>
              <ul>
             	  <li>红枣山药</li>
                <li>...</li>
              </ul>
            </li>
            <li>其他分类...</li>
          </ul>
      </div>
  </div>
  ```

- 请求并读取数据进行模版渲染

  ```javascript
  import {mapState} from 'vuex'
  export default {
    mounted () {
      // 使用 axios 请求 mockjs 提供的接口
      this.$store.dispatch('getShopGoods')
    },
    computed: {
      ...mapState(['goods'])
    }
  }
  ```

  ```html
  <li class="menu-item" v-for="(good, index) in goods" :key="index">
    <span class="text bottom-border-1px">
      <img class="icon" :src="good.icon" v-if="good.icon">
      {{good.name}}
    </span>
  </li>
  <!-- 右侧的模版渲染同上-->
  ```

### 14.5 完成用户交互功能

1. 修改当前食品分类的样式
2. 当滑动右侧食品列表时，更新左侧的当前分类
3. 点击左侧某个分类，右侧列表滑动到指定位置

```html
<!--current是标识当前分类的class 而它的值应该是一个计算后的布尔值-->
<li class="menu-item" :class="{current: index===currentIndex}" ></li>
```

> 设计计算属性：currentIndex
>
> 根据哪些数据来进行计算？
>
> 既然要实现左右两侧的联动，那么首先要获取一些位置信息。
>
> 	scrollY: 右侧食品列表滑动的Y轴坐标（滑动过程中实时变化）
>				
> 	tops: 所有右侧分类标题到屏幕顶部的距离，即li的top值组成的数组
>				
> 		（列表第一次显示后就不再变化）
>
> 1. 在滑动过程中，实时收集scrollY
> 2. 列表第一次显示后，收集tops
> 3. 实现currentIndex的计算逻辑

- 使用better-scroll实现回弹滑动

  `npm install --save better-scroll`

  [中文官网](https://ustbhuangyi.github.io/better-scroll/#/zh)

  ```javascript
  import BScroll from 'better-scroll'
  // 要考虑列表显示之后创建BScroll实例的时机
  //（参考12.2解决swiper的bug，之前使用watch和nextTick来监听数据更新）
  // 这里使用另一种方法 通过action的回调函数来通知组件数据已经更新
  mounted() {
      this.$store.dispatch('getShopGoods', () => {// 数据更新后执行
        this.$nextTick(() => { // 列表数据更新显示后执行
          new BScroll('.menu-wrapper', {
            click: true
          })
          new BScroll('.foods-wrapper', {
            click: true
          })
        })
      })
    },
  ```

  ```javascript
  //同时也要修改getShopGoods这个action
  // 异步获取商家商品列表
  async getShopGoods ({commit}, callback) {
    const result = await reqShopGoods()
    if (result.code === 0) {
      const goods = result.data
      commit(RECEIVE_GOODS, {goods})
      // 数据更新了, 通知一下组件
      callback && callback()
    }
  }
  ```

- 收集scrollY和tops

  ```javascript
  // 要收集滚动的数据，那么就要利用betterScroll对象来监听滚动事件
  
  _initScroll() {
  	// 列表显示之后创建
  	new BScroll('.menu-wrapper', {
  		click: true
  	})
      // 根据文档配置scroll选项
  	this.foodsScroll = new BScroll('.foods-wrapper', {
  		probeType: 2,  // 因为惯性滑动不会触发
  		click: true
  	})
      
  	// 给右侧列表绑定scroll监听
  	this.foodsScroll.on('scroll', ({x, y}) => {
  		console.log(x, y)
  		this.scrollY = Math.abs(y)
  	})
  } // 将其封装为初始化滚动的方法
  ```

  ```javascript
  // 初始化tops
  _initTops() {
  	// 1. 初始化tops
  	const tops = []
  	let top = 0
       // 第一个li的top为0
  	tops.push(top)
  	// 2. 收集
  	// 在foods列表下找到所有分类的li
  	const lis = this.$refs.foodsUl.getElementsByClassName('food-list-hook')
       Array.prototype.slice.call(lis).forEach(li => {
       	top += li.clientHeight
       	tops.push(top)
       })
  
       // 3. 更新数据
       this.tops = tops
  }
  ```

- 完成计算属性currentIndex的逻辑

  ```javascript
  currentIndex () { // 初始和相关数据发生了变化
  	// 得到条件数据
  	const {scrollY, tops} = this
  	// 根据条件计算产生一个结果
  	const index = tops.findIndex((top, index) => {
  	// scrollY>=当前top && scrollY<下一个top
  	return scrollY >= top && scrollY < tops[index + 1]
  	})
  	// 返回结果(也就是当前的scrollY值属于第几个li区间)
  	return index
  }
  // 此时可以实现滑动右侧列表更新左侧当前分类
  // 但如果快速滑动则会出现因为惯性滑动不能正确收集scrollY值的bug
  // 解决方法一：将probeType的值改为3 但如果不想实现监听惯性滑动触发大量事件，则需要计算滑动结束时的scrollY值来确定当前分类
  
  // 在_initScroll里给右侧列表绑定scroll结束的监听
  this.foodsScroll.on('scrollEnd', ({x, y}) => {
  	console.log('scrollEnd', x, y)
  	this.scrollY = Math.abs(y)
  })
  ```

- 实现点击左侧分类滑动右侧食物列表

  ```html
  <!--首先为li绑定click事件-->
  <li class="menu-item" :class="{current: index===currentIndex}" @click="clickMenuItem(index)"></li>
  ```

  ```javascript
  clickMenuItem (index) {
  	// 得到目标位置的scrollY
  	const scrollY = this.tops[index]
  	// 立即更新scrollY(让点击的分类项成为当前分类)
  	this.scrollY = scrollY
  	// 平滑滑动右侧列表 better-scroll里的方法
  	this.foodsScroll.scrollTo(0, -scrollY, 300)
  }
  ```


### 14.6 CartControl组件

- 完成加减选购食物的组件

  ```html
  <div class="cartcontrol">
    <!-- 有一个动画-->
    <transition name="move">
  	<div class="iconfont icon-remove_circle_outline"></div>
    </transition>
      <div class="cart-count" ></div>
      <div class="iconfont icon-add_circle"></div>
  </div>
  ```

- 分析该组件接收的props

  ```javascript
  // 组件里更改的数据分别对应每一种食物，所以不应该是简单的Number类型的count，而应该是food对象（该组件为其添加food.count属性）
  props: {
        food: Object
      },
  // 为加减按钮绑定的点击事件
  methods: {
        updateFoodCount (isAdd) {
          // 这里不能直接处理food.count因为food是在good里的数据对象
          // 应该通过vuex触发action来管理数据 并把当前的food对象参数传递过去
          this.$store.dispatch('updateFoodCount', {isAdd, food: this.food})
        }
      }
  ```

- 为food添加一个新的绑定数据count

  ```javascript
  //省略mutation-types里定义常量的过程
  
  // action中同步更新food中的count值
    updateFoodCount ({commit}, {isAdd, food}) {
      if (isAdd) {
        commit(INCREMENT_FOOD_COUNT, {food})
      } else {
        commit(DECREMENT_FOOD_COUNT, {food})
      }
    }
  
  // mutation中更改数据（注意引入vue）
  import Vue from 'vue'
  [INCREMENT_FOOD_COUNT] (state, {food}) {
      if (!food.count) { // 第一次增加
        // food.count = 1  // 这样新增的属性没有数据绑定
        /*
        对象
        属性名
        属性值
         */
        Vue.set(food, 'count', 1) // 让新增的属性也有数据绑定
      } else {
        food.count++
      }
    }
  [DECREMENT_FOOD_COUNT] (state, {food}) {
      if (food.count) { // 只有有值才去减
        food.count--
      }
    }
  ```


###  14.7 Food组件

- 点击某个食品，弹出该Food弹窗组件展示该食品的信息并可将其加入购物车

  ```javascript
  export default {
      // 要展示食品信息，所以要接收food对象的数据
      props: {
        food: Object
      },
  
      data () {
        return {
          isShow: false
        }
      },
  	
      methods: {
        // Food组件内控制显示Food组件的方法
        toggleShow () {
          this.isShow = !this.isShow
        }
      },
  
      components: {
        CartControl
      }
    }
  ```

- 在ShopGoods中引用该组件

  ```html
  <div>
      <div class="goods">
          ...
          <div class="foods-wrapper">
              ...
              <ul>
              <!--为每个食品li添加点击事件 触发显示Food弹窗-->
                  <li class="food-item"  @click="showFood(food)"></li>
              </ul>
          </div>
      </div>
      <!-- 该组件与goods同级-->
      <Food :food="food" ref="food"/>
  </div>
  ```

  > 注意每个li里的CartControl 组件都使用@click.stop 来阻止了事件冒泡，就是为了防止点击加减按钮同时触发弹出Food组件

- 同时控制是否显示Food组件(其实可以直接在Food组件上使用v-if，这里是为了练习在父组件中得到子组件对象并调用其方法)

  ```javascript
  // 显示点击的food
  showFood (food) {
    // 设置要传递给food组件的数据
    this.food = food
    // 显示food组件 (在父组件中调用子组件对象的方法)
    this.$refs.food.toggleShow()
  }
  ```

### 14.8 ShopCart组件

> 购物车组件中存放的都是count大于0的food,这些数据既可以通过vuex来管理，也可以使用computed来动态计算goods里每个food的count来管理。但computed要经过两层的轮询而且还要考虑数据量的问题，所以还是使用vuex来管理这些cartFoods数据的效率更高。

- 另外要定义几个Getters来设置购物车的一些数据信息

```javascript
export default {
  totalCount (state) {
    return state.cartFoods.reduce((preTotal, food) => preTotal + food.count, 0)
  },
  totalPrice (state) {
    return state.cartFoods.reduce((preTotal, food) => preTotal + food.count * food.price, 0)
  },
  positiveSize (state) {
    return state.ratings.reduce((preTotal, rating) => preTotal + (rating.rateType === 0 ? 1 : 0), 0)
  }
}
```

```javascript
computed: {
    // 在购物车中获取到cartFoods的state 以及商家的info
	...mapState(['cartFoods', 'info']),
	// 获取相应的Getters里的数据    
	...mapGetters(['totalCount', 'totalPrice']),
        
    // 通过计算已购食品来设置购物车不同的样式和提示文字
      payClass () {
        const {totalPrice} = this
        const {minPrice} = this.info
        return totalPrice>=minPrice ? 'enough' : 'not-enough'
      },
      payText () {
        const {totalPrice} = this
        const {minPrice} = this.info
        if(totalPrice===0) {
          return `￥${minPrice}元起送`
        } else if(totalPrice<minPrice) {
          return `还差￥${minPrice-totalPrice}元起送`
        } else {
          return '结算'
        }
      },
    },
watch: {
    totalCount: function () {
      // 如果总数量为0, 直接不显示
      if (this.totalCount === 0) {
        this.isShow = false
        // return false
      }
    },
    isShow: function () {
      if (this.isShow) {
        this.$nextTick(() => {
          // 实现BScroll的实例是一个单例
          if (!this.scroll) {
            this.scroll = new BScroll('.list-content', {
              click: true
            })
          } else {
            this.scroll.refresh() // 让滚动条刷新一下: 重新统计内容的高度
          }
        })
      }
      return this.isShow
    }
  }
```
> 注意：要保证购物车的列表是单例，不然打开多次购物车列表会初始化多个实例，然后再点击会触发多次点击事件。

- 引入mint-ui实现清空购物车的交互

  ```javascript
  // 注意CLEAR_CART的mutation不能仅仅把catFoods数组清空，还要先清空goods里food的count
  [CLEAR_CART] (state) {
      // 清除food中的count
      state.cartFoods.forEach(food => { food.count = 0 })
      // 移除购物车中所有购物项
      state.cartFoods = []
    }
  ```


### 14.9 ShopRatings组件

- 商家评价组件分为上部的Star评分以及下面的用户评价信息列表

  ```javascript
  // 先在mounted里通过触发action请求ratings数据
  mounted () {
     // 为getShopRatings这个action添加回调函数
    this.$store.dispatch('getShopRatings', () => {
      this.$nextTick(() => {
        new BScroll(this.$refs.ratings, {
          click: true
        })
      })
    })
  },
  data () {
    return {
      onlyShowText: true, // 是否只显示有文本的
      selectType: 2 // 选择的评价类型: 0满意, 1不满意, 2全部
    }
  },
  // 在computed里获取info和ratings的State数据以及返回好评数量的Getters
  computed: {
    ...mapState(['info', 'ratings']),
    ...mapGetters(['positiveSize']),
    filterRatings () {
        // 得到相关的数据
      const {ratings, onlyShowText, selectType} = this
  
        // 产生一个过滤新数组
      return ratings.filter(rating => {
        const {rateType, text} = rating
          /*
            条件1:
                selectType: 0/1/2
                rateType: 0/1
                全部 || 满意、不满意中的一种
                selectType===2 || selectType===rateType
            条件2
                onlyShowText: true/false
                text: 有值/没值
                符合条件一的全部评价 || 符合条件一的有文字的评价
                !onlyShowText || text.length>0
            */
        return (selectType === 2 || selectType === rateType) && (!onlyShowText || text.length > 0)
      })
    }
  },
  // 可以在这里自己写过滤器
  filters: {
    dateFormat:function (input) {
      var d = new Date(input);
      var year = d.getFullYear();
      var month = d.getMonth() + 1;
      var day = d.getDate() <10 ? '0' + d.getDate() : '' + d.getDate();
      var hour = d.getHours();
      var minutes = d.getMinutes();
      var seconds = d.getSeconds();
      return  year+ '-' + month + '-' + day + ' ' + hour + ':' + minutes + ':' + seconds;
    }
  }
  ```

  > filterRatings 就是为要渲染的评价列表设置各种必要条件 使其按照用户选择进行渲染（即页面显示的列表要同时满足条件一与条件二）

  ```javascript
  // filters文件夹 也可以使用moment或date-fns(推荐)库来实现日期过滤
  // npm install moment/date-fns --save
  
  import Vue from 'vue'
  // import moment from 'moment'
  import format from 'date-fns/format'
  // 自定义过滤器
  Vue.filter('date-format', function (value, formatStr = 'YYYY-MM-DD HH:mm:ss') {
    // return moment(value).format(formatStr)
    return format(value, formatStr)
  })
  
  // 在main.js文件中引入注册的过滤器
  import './filters'
  ```

### 14.10 ShopInfo组件

- 获取商家列表后就已经得到了商家信息info数据，但是还没有创建ShopInfo组件对象

  ```javascript
  // mounted之后创建BScroll对象
  mounted () {
    // 如果数据还没有, 直接结束
    if (!this.info.pics) {
      return
    }
    // 数据有了, 可以创建BScroll对象形成滑动
    this._initScroll()
  }
  ```

- 同时要动态计算横向滑动的ul宽度

  ```javascript
  methods: {
    _initScroll () {
      new BScroll('.shop-info')
      // 动态计算ul的宽度
      const ul = this.$refs.picsUl
      const liWidth = 120
      const space = 6
      const count = this.info.pics.length
      ul.style.width = (liWidth + space) * count - space + 'px'
  
      new BScroll('.pic-wrapper', {
        scrollX: true // 水平滑动
      })
    }
  }
  ```

## 15. Search组件

- 既然要实现搜索功能，那么就要有搜索请求的接口以及vuex数据

  ```javascript
  // api/index.js里 添加根据经纬度和关键字搜索商铺列表的接口
  export const reqSearchShop = (geohash, keyword) => ajax(BASE_URL+'/search_shops', {geohash, keyword})
  ```

  ```javascript
  // 异步获取商家商品列表的action
  async searchShops ({commit, state}, keyword) {
    const geohash = state.latitude + ',' + state.longitude
    const result = await reqSearchShop(geohash, keyword)
    if (result.code === 0) {
      const searchShops = result.data
      commit(RECEIVE_SEARCH_SHOPS, {searchShops})
    }
  }
  ```

- 在Search组件中触发action

  ```javascript
  search () {
      // 得到搜索关键字
      const keyword = this.keyword.trim()
      // 进行搜索
      if (keyword) {
        this.$store.dispatch('searchShops', keyword)
      }
    }
  ```

- 通过router-link将搜索结果searchShops渲染出来

  ```html
  <router-link :to="{path:'/shop', query:{id:item.id}}" tag="li"
   v-for="item in searchShops" :key="item.id" class="list_li">
      ...
  </router-link>
  ```


## 16. 项目优化/扩展

### 16.1 缓存路由组件对象

````html
<!-- 好处: 复用路由组件对象, 复用内存中路由组件获取的后台数据 -->
<keep-alive>
	<router-view />
</keep-alive>
````

### 16.2 路由组件懒加载

> 我们写的所有Js文件最后都会打包成一个文件，而我们实际的需求是路由组件并不是一次全部加载过来，而是按需加载。所以就要在打包前就对代码进行分割，从而实现路由组件懒加载。

```javascript
// router/index.js文件中 改变引入方式 实现路由组件懒加载
const Msite = () => import('../pages/Msite/Msite.vue')
const Search = () => import('../pages/Search/Search.vue')
const Order = () => import('../pages/Order/Order.vue')
const Profile = () => import('../pages/Profile/Profile.vue')

// 此时的Msite等都是返回路由组件的函数，只有请求对应的路由路径时(第一次)才会执行此函数并加载路由组件
```

> 此时切换路由，可以在控制台NetWork里看到拆分打包后的js文件实现了按需加载

### 16.3 图片懒加载: vue-lazyload

安装`npm install --save vue-loader`

```javascript
// 在main.js文件中引入并注册插件
import VueLazyload from 'vue-lazyload'
// 将一张loading图片加载进来
import loading from './common/img/loading.gif'
Vue.use(VueLazyload, { // 内部自定义一个指令lazy
	loading
})
// 在图片标签中使用 （Food组件）
<img v-lazy="food.image">
```

### 16.4 打包文件分析与优化

- vue 脚手架提供了一个用于可视化分析打包文件的包 webpack-bundle-analyzer 和配置
- 启用打包可视化: `npm run build --report` 可以根据可视化文件分析页面对项目进行优化