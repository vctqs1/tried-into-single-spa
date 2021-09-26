# tried-into-single-spa


## Table of Contents

1.  [Running Locally](#running-locally)
2.  [Purpose of the project](#purpose-of-the-project)
3.  [Adding new packages](#adding-new-packages)
4.  [Root Project](#what-is-hoisting)

## Running Locally

```bash
$ git clone git@github.com:vctqs1/tried-into-single-spa.git
$ cd tried-into-single-spa
$ cd root-config
$ yarn
$ yarn start-all
$ open http://localhost:9000/
```
When you see this image, you start succesfully ![image](https://user-images.githubusercontent.com/30227910/134805976-8814a95d-1264-416a-a8f4-7210c6e13ea1.png)

## Purpose of the project

Tried into demonstrate **Micro Frontends**. 
- Share layout between pages
- Share logic between different frameworks: React,VueJS

## Adding new packages

Follow the instructions:

`npx create-single-spa`
Then,

```bash
? Directory for new project .
? Select type to generate single-spa application / parcel
? Which framework do you want to use? react
? Which package manager do you want to use? yarn
? Will this project use Typescript? Yes
? Organization name (can use letters, numbers, dash or underscore) vctqs1
? Project name (can use letters, numbers, dash or underscore) [react-*|vue-*]
```

## Root Project

**app-root** consist of root-config and index.ejs. In order to add new packages into single-spa, first register your applications into root-config like this:

```typescript
registerApplication({
    name: "@vctqs1/react-navbar",
    app: () => System.import("@vctqs1/react-navbar"),
    activeWhen: ["/"],
});
registerApplication({
    name: "@vctqs1/react-home",
    app: () => System.import("@vctqs1/react-home"),
    activeWhen: ["/"],
});
start({
  urlRerouteOnly: true,
});
```

Then, add them into your **importmap**:

```html
<% if (isLocal) { %>
<script type="systemjs-importmap">
    {
      "imports": {
        "react": "https://cdn.jsdelivr.net/npm/react@16.13.1/umd/react.development.js",
        "react-dom": "https://cdn.jsdelivr.net/npm/react-dom@16.13.1/umd/react-dom.development.js",
        "@vctqs1/root-config": "http://localhost:9000/vctqs1-root-config.js",
        "@vctqs1/react-navbar": "http://localhost:9001/vctqs1-react-navbar.js",
        "@vctqs1/react-home": "http://localhost:9002/vctqs1-react-home.js"
      }
    }
</script>
<% } %>
```

Organization name -- **@vctqs1** -- and packages name -- **vctqs1-new-package** -- carries great importance, if you mess one of them project will complain.

I was following this [article](https://ogzhanolguncu.com/blog/migrating-cra-to-micro-frontends-with-single-spa) to setup this project
