---
title: "anonymous-santa"
date: 2020-12-16 01:36:12 
categories: web vue firebase sideproject
---

vue + firebase

### Install vue-cli and vuefire 
1. Install vue-cli (ver3)
```
➜  anonymous-santa npm install -g @vue/cli
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated fsevents@1.2.13: fsevents 1 will break on node v14+ and could be using insecure binaries. Upgrade to fsevents 2.
npm WARN deprecated chokidar@2.1.8: Chokidar 2 will break on node v14+. Upgrade to chokidar 3 with 15x less dependencies.
npm WARN deprecated urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
npm WARN deprecated resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
npm WARN deprecated @hapi/topo@3.1.6: This version has been deprecated and is no longer supported or maintained
npm WARN deprecated @hapi/bourne@1.3.2: This version has been deprecated and is no longer supported or maintained
npm WARN deprecated @hapi/address@2.1.4: Moved to 'npm install @sideway/address'
npm WARN deprecated @hapi/hoek@8.5.1: This version has been deprecated and is no longer supported or maintained
npm WARN deprecated @hapi/joi@15.1.1: Switch to 'npm install joi'

added 1357 packages, and audited 1357 packages in 38s

54 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

2. Create vue project
```
➜  anonymous-santa vue create santa


Vue CLI v4.5.9
? Please pick a preset: Default ([Vue 2] babel, eslint)


Vue CLI v4.5.9
✨  Creating project in /Users/brie/Documents/study/web/anonymous-santa/santa.
🗃  Initializing git repository...
⚙️  Installing CLI plugins. This might take a while...


added 1269 packages, and audited 1269 packages in 26s

62 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
🚀  Invoking generators...
📦  Installing additional dependencies...


added 49 packages, and audited 1318 packages in 3s

66 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
⚓  Running completion hooks...

📄  Generating README.md...

🎉  Successfully created project santa.
👉  Get started with the following commands:

 $ cd santa
 $ npm run serve
```

3. Add vuefire
```
➜  anonymous-santa cd santa
➜  santa git:(master) yarn add vuefire firebase
```

4. Import vuefire inside a src/main.js file.
-> https://stackoverflow.com/questions/56416834/export-default-imported-as-vuefire-was-not-found-in-vuefire
```
// main.js

import Vue from 'vue'
import { firestorePlugin } from 'vuefire'
import App from './App.vue'

Vue.use(firestorePlugin)
Vue.config.productionTip = false

new Vue({
  render: h => h(App)
}).$mount('#app')
```

### Create three components
1. Include Bootstrap4
```
➜  santa git:(master) ✗ yarn add bootstrap
```

2. Include inside src/App.vue  file.
```
<style lang="css">
  @import '../node_modules/bootstrap/dist/css/bootstrap.min.css';
</style>
```

3. Inside src/components folder, create the following three files.
4. Step 4: Install and configure the router.
```
➜  santa git:(master) ✗ yarn add vue-router
```

Step 6: Add Routing Progress bar Indicator.
```
import Vue from 'vue'
import { firestorePlugin } from 'vuefire'
import VueRouter from 'vue-router'
import App from './App.vue'

import AddItem from './components/AddItem.vue'
import EditItem from './components/EditItem.vue'
import ListItem from './components/ListItem.vue'
import Home from './components/Home.vue'

import NProgress from "nprogress";

Vue.use(firestorePlugin)
Vue.use(VueRouter)
Vue.use(NProgress)
Vue.config.productionTip = false

const routes = [
  {
    name: 'Home',
    path: '/',
    component: Home
  },
  {
        name: 'Add',
        path: '/add',
        component: AddItem
  },
  {
      name: 'Edit',
      path: '/edit/:id',
      component: EditItem
  },
  {
      name: 'List',
      path: '/index',
      component: ListItem
  },
];

const router = new VueRouter({ mode: 'history', routes: routes });

router.beforeResolve((to, from, next) => {
  if (to.name) {
      NProgress.start()
  }
  next()
})

router.afterEach(() => {
  NProgress.done()
})

new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

## hosting
```
➜  santa git:(master) ✗ firebase init hosting

     ######## #### ########  ######## ########     ###     ######  ########
     ##        ##  ##     ## ##       ##     ##  ##   ##  ##       ##
     ######    ##  ########  ######   ########  #########  ######  ######
     ##        ##  ##    ##  ##       ##     ## ##     ##       ## ##
     ##       #### ##     ## ######## ########  ##     ##  ######  ########

You're about to initialize a Firebase project in this directory:

  /Users/brie/Documents/study/web/anonymous-santa/santa


=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

? Please select an option: Use an existing project
? Select a default Firebase project for this directory: anonymous-santa (anonymous-santa)
i  Using project anonymous-santa (anonymous-santa)

=== Hosting Setup

Your public directory is the folder (relative to your project directory) that
will contain Hosting assets to be uploaded with firebase deploy. If you
have a build process for your assets, use your build's output directory.

? What do you want to use as your public directory? public
? Configure as a single-page app (rewrite all urls to /index.html)? No
? Set up automatic builds and deploys with GitHub? No
✔  Wrote public/404.html
? File public/index.html already exists. Overwrite? No
i  Skipping write of public/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

✔  Firebase initialization complete!
➜  santa git:(master) ✗ firebase deploy --only hosting

=== Deploying to 'anonymous-santa'...

i  deploying hosting
i  hosting[anonymous-santa]: beginning deploy...
i  hosting[anonymous-santa]: found 3 files in public
✔  hosting[anonymous-santa]: file upload complete
i  hosting[anonymous-santa]: finalizing version...
✔  hosting[anonymous-santa]: version finalized
i  hosting[anonymous-santa]: releasing new version...
✔  hosting[anonymous-santa]: release complete

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/anonymous-santa/overview
Hosting URL: https://anonymous-santa.web.app
```

# 참고 자료
- https://niceman.tistory.com/category/%EC%9B%B9%20%ED%94%84%EB%A1%A0%ED%8A%B8/Vue
- https://github.com/KrunalLathiya/VueFirebaseTutorial
- https://appdividend.com/2018/04/21/vue-firebase-crud-example/
- https://sabe.io/tutorials/building-note-taking-app-vue-js-firebase?fbclid=IwAR3L9kYFjC8WjhdLWK1OlamPQE4kTk3_pVqpTN_HmYi-sBOII0O8lqdVmKA#firebase-real-time-database
- https://beomy.tistory.com/40

## 학습 자료
- https://vuex.vuejs.org/kr/
- https://gitct.goorm.io/learn/lecture/24989/%EB%B6%80%EB%8B%B4-%EC%97%86%EC%9D%B4-%EC%89%BD%EA%B3%A0-%ED%8E%B8%ED%95%98%EA%B2%8C-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B8%B0%EC%B4%88-%EC%A0%95%EB%A6%AC
- https://romeoh.tistory.com/entry/Firebase-Python-Firebase-Realtime-Database
- https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
- https://ko.javascript.info/arrow-functions-basics
- https://kr.vuejs.org/v2/guide/
- https://jeonghwan-kim.github.io/2018/04/07/vue-router.html
- https://m.blog.naver.com/PostView.nhn?blogId=cosmosjs&logNo=221231207526&proxyReferer=https:%2F%2Fwww.google.com%2F
