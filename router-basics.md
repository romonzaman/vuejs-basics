# Vue Router
## The official Router for Vue.js

##### Installation
```
npm install vue-router@4
```


router file
>router/index.js

```js
import { createWebHistory, createRouter } from "vue-router"

import ProductList from "../components/ProductList.vue"
import ProductDetails from "../components/ProductDetails.vue"

const routes = [
    {
        path: "/",
        component: ProductList,
    },
    {
        path: "/product/:id",
        component: ProductDetails
    },

]

const router = createRouter({
    history: createWebHistory(import.meta.env.BASE_URL),
    routes
})

export default router
```

> main.js

```js
import { createApp } from 'vue'
import App from './App.vue'

import router from './router/'

createApp(App).use(router).mount('#app')
```


> from template

```js
$route.params.id
```

- from script

```js
import { useRoute } from 'vue-router';
const router = useRoute()
const product_id = router.params.id
```

- from script redirect
```js
import { RouterLink, useRouter } from 'vue-router';
const router = useRouter()
router.push({ path: '/product/' + id })
```

```js
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>

    <router-view></router-view>
```


- routes
```js
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
]
```

```js
  // dynamic segments start with a colon
  { path: '/users/:id', component: User },
```

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>',
}
```


```js
const User = {
  template: '...',
  async beforeRouteUpdate(to, from) {
    // react to route changes...
    this.userData = await fetchUser(to.params.id)
  },
}
```

```js
const routes = [
  // will match everything and put it under `$route.params.pathMatch`
  { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  // will match anything starting with `/user-` and put it under `$route.params.afterUser`
  { path: '/user-:afterUser(.*)', component: UserGeneric },
]
```

- Optional parameters
- You can also mark a parameter as optional by using the ? modifier (0 or 1):

```js
const routes = [
  // will match /users and /users/posva
  { path: '/users/:userId?' },
  // will match /users and /users/42
  { path: '/users/:userId(\\d+)?' },
]
```
- Note that * technically also marks a parameter as optional but ? parameters cannot be repeated.



```js
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      {
        // UserProfile will be rendered inside User's <router-view>
        // when /user/:id/profile is matched
        path: 'profile',
        component: UserProfile,
      },
      {
        // UserPosts will be rendered inside User's <router-view>
        // when /user/:id/posts is matched
        path: 'posts',
        component: UserPosts,
      },
    ],
  },
]
```

```js
router.push({ path: '/home', replace: true })
// equivalent to
router.replace({ path: '/home' })
```



```js
// go forward by one record, the same as router.forward()
router.go(1)

// go back by one record, the same as router.back()
router.go(-1)

// go forward by 3 records
router.go(3)

// fails silently if there aren't that many records
router.go(-100)
router.go(100)
```

#### Named Views

```html
<router-view class="view left-sidebar" name="LeftSidebar"></router-view>
<router-view class="view main-content"></router-view>
<router-view class="view right-sidebar" name="RightSidebar"></router-view>
```

```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: '/',
      components: {
        default: Home,
        // short for LeftSidebar: LeftSidebar
        LeftSidebar,
        // they match the `name` attribute on `<router-view>`
        RightSidebar,
      },
    },
  ],
})
```

