
## Define Store

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({ count: 0, name: 'Eduardo' }),
  getters: {
    doubleCount: (state) => state.count * 2,
  },
  actions: {
    increment() {
      this.count++
    },
  },
})
```


```js

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const name = ref('Eduardo')
  const doubleCount = computed(() => count.value * 2)
  function increment() {
    count.value++
  }

  return { count, name, doubleCount, increment }
})

```


***In Setup Stores:***

- ref()s become state properties
- computed()s become getters
- function()s become actions


## Using store
```js
<script setup>
import { useCounterStore } from '@/stores/counter'

// access the `store` variable anywhere in the component ✨
const store = useCounterStore()
</script>

```


#### Accessing the state
```js
const store = useStore()
store.count++
```

```js
//In Option Stores
const store = useStore()
store.$reset()

//In Setup Stores
export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)

  function $reset() {
    count.value = 0
  }

  return { count, $reset }
})
```

#### Passing arguments to getters
```js
//define
export const useStore = defineStore('main', {
  getters: {
    getUserById: (state) => {
      return (userId) => state.users.find((user) => user.id === userId)
    },
  },
})
//usage
<script setup>
import { storeToRefs } from 'pinia'
import { useUserListStore } from './store'

const userList = useUserListStore()
const { getUserById } = storeToRefs(userList)
// note you will have to use `getUserById.value` to access
// the function within the <script setup>
</script>

<template>
  <p>User 2: {{ getUserById(2) }}</p>
</template>

```


```js
import { mande } from 'mande'

const api = mande('/api/users')

export const useUsers = defineStore('users', {
  state: () => ({
    userData: null,
    // ...
  }),

  actions: {
    async registerUser(login, password) {
      try {
        this.userData = await api.post({ login, password })
        showTooltip(`Welcome back ${this.userData.name}!`)
      } catch (error) {
        showTooltip(error)
        // let the form component display the error
        return error
      }
    },
  },
})
```

