#### vue路由文件模块化，vue路由文件拆分

路由信息非常多的情况下，如果将路由配置信息都放在一个文件里面，那么这个JS是不方便维护的，

那么，这个时候需要我们把这个庞大的路由文件，根据项目功能分类，拆分为几个不同的路由文件。

直接贴代码

1.文件`router/index.js` (将system和tool的路由拆分到单独的文件)

```javascript
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)
import Layout from '@/layout'
// 将system和tool的路由拆分到单独的文件
import systemRoute from './module/system'
import toolRoute from './module/tool'

// 公共路由
export const constantRoutes = [
  {
    path: '/redirect',
    component: Layout,
    hidden: true,
    children: [
      {
        path: '/redirect/:path(.*)',
        component: () => import('@/views/redirect')
      }
    ]
  },
  {
    path: '/login',
    component: () => import('@/views/login'),
    hidden: true
  }
]

// 动态路由，基于用户权限动态去加载
export const dynamicRoutes = [
  //拆分的路由文件在这里加载
  systemRoute,
  toolRoute
]

export default new Router({
  mode: 'history', // 去掉url中的#
  scrollBehavior: () => ({ y: 0 }),
  routes: constantRoutes
})

```

2. 文件`router/module/system.js`

```javascript
import Layout from '@/layout'

const routes = [
  {
    path: '/system/user-auth',
    component: Layout,
    hidden: true,
    permissions: ['system:user:edit'],
    children: [
      {
        path: 'role/:userId(\\d+)',
        component: () => import('@/views/system/user/authRole'),
        name: 'AuthRole',
        meta: { title: '分配角色', activeMenu: '/system/user' }
      }
    ]
  }
]
export default routes

```

3. 文件`router/module/tool.js`

   ```javascript
   import Layout from '@/layout'
   
   const routes = [
     {
       path: '/tool/gen-edit',
       component: Layout,
       hidden: true,
       permissions: ['tool:gen:edit'],
       children: [
         {
           path: 'index',
           component: () => import('@/views/tool/gen/editTable'),
           name: 'GenEdit',
           meta: { title: '修改生成配置', activeMenu: '/tool/gen' }
         }
       ]
     }
   ]
   export default routes
   
   ```

   