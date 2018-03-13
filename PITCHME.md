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

```vue
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

```vue
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

```vue
Hi {{ username.toUpperCase() }}, hope you have a nice day!
```

---

### Transform data on they fly

using method

```vue
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

```vue
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

Lets make it look more nice!

- grids, rows
- vuetify.js
- bootstrap, ...

---


```vue
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

```vue
<div v-if="username !== ''">
  Hi {{ upperCaseUsername }}, hope you have a nice day!
</div>
<div v-else>
  Please specify your username
</div>
```

---


### Lets talk about Components

 - abstraction that allows us to build large-scale applications
 - composed of small, self-contained, and often reusable components

![Components](https://github.com/JonasMunck/vuejs-b2s/raw/master/components.png)


---
### Add Greeting component

```
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

---

```
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

```
<!-- in parent -->
<greeting username="Jonas">
```

To make it dynamic

```
<!-- in parent -->
<greeting :username="Jonas">
```


---

unit test

---

### The karma API

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

```
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

routing

---

```
npm install --save axios
```

---
