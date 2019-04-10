1、vue的核心思想 
结合周边的生态构建的渐进式的框架
- 声明式渲染：new view 实例，再模版里使用
- 组件系统：自定义组件
- 客户端路由：引入view route插件
- 大规模状态管理： 多个组件间不能直接统计，vuex将共享信息抽离供所有组件使用
- 构建工具：webpack
核心思想：
- 数据驱动：传统js中需要js操作dom.但vue中js查询数据放入model model和dom交互由模版组件管理,通过Object.defineProperty()扩展model的属性,定义属性的get和set方法，当给该属性赋值时会调用属性的set方法，在set方法中处理写回dom
- 组件化：抽离公用的部分作为一个组件，页面可以看成一个组件树
MVVM
- view(DOM)-ViewModel(vue,处理DOM和Model的双向同步)-Model(js pojo对象)
2、vue+nodejs+mongondb项目
- 主要 商城系统
- 功能 商品列表 订单 登录 结算
- 技术栈 前端vue+ES6 后端 node+express 数据库 mongondb
