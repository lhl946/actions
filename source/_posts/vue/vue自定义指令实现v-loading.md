---
title: vue自定义指令实现v-loading
date: 2020-07-07 14:39:45
categories: 
    - vue
tags: 
    - vue
    - loading
    - 自定义指令
cover: http://pic.w2bc.com/upload/201609/24/201609241119481550.jpg
---

# vue自定义指令实现v-loading

## 目录结构

- loading/index.js
- loading/Loading.vue
- loading/loading.js

## loading.vue

```vue
<template>
  <div v-show="visible" class="loading-wrap">
    <ul class="loading-box">
        <Spin></Spin>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      visible: false
    }
  }
}
</script>
<style scoped>
.loading-wrap {
  position: absolute;
  top: 0;
  left: 0;
  width: 1726px;
  height: 747px;
  background: rgba(255, 255, 255, 0.5);
  z-index: 999;
}
.loading-box {
  position: absolute;
  left: 50%;
  top: 50%;
  height: 60px;
  width: 60px;
  margin-top: -30px;
  margin-left: -30px;
}
</style>
```



## loading.js

```js
import Vue from "vue";
import Loading from "./Loading.vue";

const Mask = Vue.extend(Loading);

const toggleLoading = (el, binding) => {
  console.log("Mask", Mask);

  if (binding.value) {
    Vue.nextTick(() => {
      // 控制loading组件显示
      el.instance.visible = true;
      // 插入到目标元素
      insertDom(el, el, binding);
    });
  } else {
    el.instance.visible = false;
  }
};

const insertDom = (parent, el) => {
  parent.appendChild(el.mask);
};

export default {
  inserted(el, binding) {
    const width = window.getComputedStyle(el).width;
    const height = window.getComputedStyle(el).height;
    const mask = new Mask({
      el: document.createElement("div"),
      data() {}
    });
    mask.$el.style.width = width;
    mask.$el.style.height = height;
    el.instance = mask;
    el.mask = mask.$el;
    el.maskStyle = {};
    el.style.position = "relative";
    binding.value && toggleLoading(el, binding);
  },
  update: function(el, binding) {
    if (binding.oldValue !== binding.value) {
      toggleLoading(el, binding);
    }
  },
  unbind: function(el, binding) {
    el.instance && el.instance.$destroy();
  }
};

```

## index.js

```
import loading from './loading'
export default {
  install (Vue) {
    Vue.directive('loading', loading)
  }
}
```

## 使用

在main.js中引入

```
import loading from './components/loading'
Vue.use(loading)
```

组件内使用

```
<div v-loading="loading"></div>
```

```
data() {
    return {
        loading:true
    }
},
```

```
mounted(){
  setTimeout(() => {
      this.loading = false;
  }, 1500);
},
```

