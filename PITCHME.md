# Vue.js


---

- Build and develop with Webpack
- live coding and unit testing (we will spend most time here)
- Quick view on `***` management tool for more components

---

Requirements

- Node (v8+) and npm
- bash-like terminal (git bash)

https://nodejs.org/en/download/

---

### What is even node and npm


> Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.

---

#### npm

> npm is the package manager for JavaScript and the world’s largest software registry. Discover packages of reusable code — and assemble them in powerful new ways.

- just as `pip` (python), `maven` (java), `nuget` (C#), ...

---

### Installing vue


```bash
npm install -g vue-cli
```

- [vue-cli](https://github.com/vuejs/vue-cli) is a tool that helps you construct project setups with all kinds project templates

```bash
vue init webpack vue-demo
```

- generates folder `vue-demo` with template `webpack`

---

### Hello world

<!-- git co v1 -->

- npm run dev

- Runs development server
- hot reload, linting, syntax errors
- webpage runs on http://localhost:8080

---

### Reactive bindings

```html
<template>
  <div>{{msg}}</div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello World!'
    }
  },
}
</script>

```

---


---

```html
<template>
  <v-app>
    <v-container>

      <v-text-field
        label="Username"
        v-model="username" />

      Hi {{ username }}, hope you have a nice day!

    </v-container>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello World!',
      username: ''
    }
  },
}
</script>
```

---

### Transform data on they fly

direct in the template

```
Hi {{ username.toUpperCase() }}, hope you have a nice day!
```

---

### Transform data on they fly

using method

```html
<template>
  <v-app>
    <v-container>

      <v-text-field
        label="Username"
        v-model="username" />

      Hi {{ transformUsername() }}, hope you have a nice day!

    </v-container>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello World!',
      username: ''
    }
  },

  methods: {
    transformUsername () {
      return this.username.toUpperCase()
    }
  }
}
</script>
```

---

### Transform data on they fly

using computed - the correct way

```html
<template>
  <v-app>
    <v-container>

      <v-text-field
        label="Username"
        v-model="username" />

      Hi {{ transformUsername() }}, hope you have a nice day!

    </v-container>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello World!',
      username: ''
    }
  },

  computed: {
    transformUsername () {
      return this.username.toUpperCase()
    }
  }
}
</script>
```
---

Recap

- v-model to bind template <-> model
- computed for pure transformations, when data on instance changes.
- we will see correct usage of `methods` later


---

Lets make it look more nice!

- frontend development is so much more than angular, react, elm, vue

- layout, buttons, forms, spinners, modals, ...

Note:
open vuetify, go to https://vuetifyjs.com/en/components/alerts and copy paste the success alert into the code.

Then talk about the GRID

---


```html
<template>
  <v-app>
    <v-container>
      <v-layout row>
        <v-flex xs2>

          <!-- where things happen -->
          <v-text-field
            label="Username"
            v-model="username" />

          Hi {{ upperCaseUsername }}, hope you have a nice day!

          <!-- end of where things happen -->
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>
```

---

Conditional rendering

```html
<div v-if="username !== ''">
  Hi {{ upperCaseUsername }}, hope you have a nice day!
</div>
<div v-else>
  Please specify your username
</div>
```

Note:
There is no UI without UX - the experience should be good for the user. Don't say hi if no username has been entered.

---


### Lets talk about Components

 - abstraction that allows us to build large-scale applications
 - composed of small, self-contained, and often reusable components


---

![Components](https://github.com/JonasMunck/vuejs-b2s/blob/master/img/components.PNG)


---
### Add Greeting component

```html
<!-- @/components/Greeting.vue -->
<template>
  <div v-if="username !== ''">
    Hi {{ username }}, hope you have a nice day!
  </div>
  <div v-else>
    Please specify your username
  </div>
</template>

<script>
export default {

}
</script>

<style>

</style>
```

Note:
before adding component: run
git co -f 35eed81

---

### In parent

```html
<template>
<!-- ... more code --->

<greeting />

<!-- ... more code --->
</template>

<script>
import Greeting from '@/components/Greeting'
export default {
  components: {
    Greeting
  },

  <!-- ... more code --->
```

---


### Pass data to child components - props

- Every component instance has its own isolated scope
- cannot directly reference parent data in a child component

---

```js
<script>
export default {
  props: ['username']
}
</script>
```

---

```html
<!-- in parent -->
<greeting username="Jonas">
```

To make it dynamic

```html
<!-- in parent -->
<greeting :username="username">
```


---

unit test

---

### JS Unit test - syntax

```js
describe('Greeting.vue', () => {
  it('should render correct contents', () => {
    // test implementation here
  })
})
```

---

### Test the greeting component

```js
describe('Greeting.vue', () => {
  it('should render correct contents', () => {
    const Constructor = Vue.extend(Greeting)
    const vm = new Constructor().$mount()
    expect(vm.$el.textContent)
      .toContain('Please specify your username')
  })
})
```

---

```bash
$ npm run unit
```


```bash
Greeting.vue › should render correct contents

expect(string).toContain(value)

Expected string:
    "
    Hi , hope you have a nice day!
"
To contain value:
    "Please specify your username"
```

---

### Fix broken test

```html
<template>
  <div v-if="validUsername">
    Hi {{ username }}, hope you have a nice day!
  </div>
  <div v-else>
    Please specify your username
  </div>
</template>

<script>
export default {
  props: ['username'],

  computed: {
    validUsername () {
      return this.username && this.username.length > 0
    }
  }
}
</script>

<style>

</style>
```

@[2]
@[14-18]

---

## It should say hi to user

```js
it('should say hi to user', () => {
  const Constructor = Vue.extend(Greeting)
  const vm = new Constructor({propsData: {username: 'Jonas'}}).$mount()
  expect(vm.$el.textContent)
  .toContain('Hi Jonas, hope you have a nice day!')
})
```

Note:
Run `npm run unit` after adding test

---

## 2 seconds pause

Any Questions?

---

## Next Step - Routing

- routing framework is included in the webpack template
- maps a url path to a container
- inject router in top component

```html
<!-- src/App.vue> -->
<router-view />
```

---


## Project structure


```
src
  assets
  components
    global components, like notifications
  pages
    home
      Home.vue <- the main file
      components
        specific only for home page
      pages
        subpages to home
        can also have components
    another-top-page
```

@[3-4]
@[5-13]
@[6-12]
@[7]
@[8-9]
@[10-12]


Source: https://medium.com/@alexmngn/how-to-better-organize-your-react-applications-2fd3ea1920f1

---

### Convert existing code to use router

Back to the code

---



## App.vue

```html
<template>
  <v-app>
    <router-view />
  </v-app>
</template>

<script>

</script>
```

---


#### src/pages/home/Home.vue

```html
<template>
  <v-container>
    <v-layout row>
      <v-flex xs2>

        <!-- where things happen -->
        <v-text-field
          label="Username"
          v-model="username" />
        <!-- end of where things happen -->
        <greeting v-bind:username="username"/>

      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import Greeting from '@/components/Greeting'
export default {
  components: {
    Greeting
  },

  data () {
    return {
      msg: 'Hello World!',
      username: ''
    }
  }
}
</script>
```

---

#### src/nextlevel/NextLevel.vue

```html
<template>
  <v-container>
    <v-layout row>
      <div class="display-1">Next level</div>
    </v-layout>
  </v-container>
</template>

<script>
export default {

}
</script>

<style>

</style>
```

---


#### src/pages/router/index.js

```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'home',
      component: require('@/pages/home/Home').default
    },
    {
      path: '/next-level',
      name: 'next-level',
      component: require('@/pages/nextlevel/NextLevel').default
    }
  ]
})
```

---

```html
<v-btn
  @click="navToNextLevel"
>
  To Next level
</v-btn>
```

```js
methods: {
    navToNextLevel () {
      this.$router.push({name: 'next-level'})
    }
  }
```


---

## Advanced routing

- Nested views for advanced layout

https://router.vuejs.org/en/essentials/named-views.html

- Guards for doing f.ex permission based checks

https://router.vuejs.org/en/advanced/navigation-guards.html


---

HTTP

```
npm install --save axios
```

---

```js
import axios from 'axios'

Vue.use(Vuetify)

Vue.config.productionTip = false

let axiosInstance = axios.create({
  baseURL: 'http://localhost:8000'
})

Vue.http = Vue.prototype.$http = axiosInstance
```

@[1]
@[7-11]

---

##### Fetch data

Add method for fetching data

```js
  methods: {
    async fetchPosts () {
      let response = await this.$http.get('https://jsonplaceholder.typicode.com/posts')
      console.log(response)
      return response.data
    }
  }
```

---

Another lifecycle event - **created**

```js
  async created () {
    this.posts = await this.fetchPosts()
  },
```

---

Render posts in template

```html
<template>
  <v-container>
    <v-layout row>
      <div class="display-1">Next level</div>
      {{ posts }}
    </v-layout>
  </v-container>
</template>
```

---

A Components for posts

_@/src/pages/nextlevel/components/Posts.vue_

```html
<template>
  <div>{{posts}}</div>
</template>

<script>
export default {
  props: ['posts']
}
</script>

<style>

</style>
```

---


A post component

```html
<template>
  <v-card>
    <v-card-title primary-title>
      <h3 class="headline mb-0">{{ post.title }}</h3>
    </v-card-title>
    <v-card-text>{{post.body}}</v-card-text>
  </v-card>
</template>

<script>
export default {
  props: ['post']
}
</script>

<style>

</style>
```

---
