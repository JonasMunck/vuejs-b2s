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


> Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient


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

Runs development server
Very convinient, webpack has hot reload

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

Lets make it look more nice!

- grids, rows
- vuetify.js
- bootstrap, ...

---

Conditional rendering

with error, use chrome debugger

---

props

---

unit test

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

routing

---

```
npm install --save axios
```

---
