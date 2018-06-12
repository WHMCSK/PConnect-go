# PConnect

## What it contains?

* server side render via [goja](https://github.com/dop251/goja)
* api requests between your react application and server side application directly  via [fetch polyfill](https://github.com/olebedev/gojax/tree/master/fetch)
* title, Open Graph and other domain-specific meta tags render for each page at the server and at the client
* server side redirect
* embedding static files into artefact via bindata
* high performance [echo](https://github.com/labstack/echo) framework
* advanced cli via [cli](https://github.com/codegangsta/cli)
* Makefile based project
* one(!) terminal window process for development
* routing via [react-router](https://github.com/reactjs/react-router)
* ES6 & JSX via [babel-loader](https://github.com/babel/babel-loader) with minimal runtime dependency footprint
* [redux](https://rackt.org/redux/) as state container
* [redux-devtools](https://github.com/gaearon/redux-devtools)
* css styles without global namespace via PostCSS, [css-loader](https://github.com/webpack/css-loader) & css-modules
* separate css file to avoid FOUC
* hot reloading via [react-transform](https://github.com/gaearon/babel-plugin-react-transform) & [HMR](http://webpack.github.io/docs/hot-module-replacement.html)
* webpack bundle builder
* eslint and golint rules for Makefile

## Workflow dependencies

* [golang](https://golang.org/)
* [node.js](https://nodejs.org/) with [yarn](https://yarnpkg.com)
* [GNU make](https://www.gnu.org/software/make/)

Note that probably not works at windows.

## Project structure

##### The server's entry point
```
$ tree server
server
├── api.go
├── app.go
├── bindata.go <-- this file is gitignored, it will appear at compile time
├── conf.go
├── data
│   └── templates
│       └── react.html
├── main.go <-- main function declared here
├── react.go
└── utils.go
```

The `./server/` is flat golang package.

##### The client's entry point

It's simple React application

```
$ tree client
client
├── actions.js
├── components
│   ├── app
│   │   ├── favicon.ico
│   │   ├── index.js
│   │   └── styles.css
│   ├── homepage
│   │   ├── index.js
│   │   └── styles.css
│   ├── not-found
│   │   ├── index.js
│   │   └── styles.css
│   └── usage
│       ├── index.js
│       └── styles.css
├── css
│   ├── funcs.js
│   ├── global.css
│   ├── index.js
│   └── vars.js
├── index.js <-- main function declared here
├── reducers.js
├── router
│   ├── index.js
│   ├── routes.js
│   └── toString.js
└── store.js
```

The client app will be compiled into `server/data/static/build/`.  Then it will be embedded into go package via _go-bindata_. After that the package will be compiled into binary.

**Convention**: javascript app should declare [_main_](https://github.com/olebedev/go-starter-kit/blob/master/client/index.js#L4) function right in the global namespace. It will used to render the app at the server side.

## Install

Clone the repo:

```
$ git clone https://github.com/JINSCOP/PConnect.git $GOPATH/src/github.com/<username>/<project>
$ cd $GOPATH/src/github.com/<username>/<project>
```

Install dependencies:

```
$ make install
```

## Run development

Start dev server:

```
$ make serve
```

that's it. Open [http://localhost:5001/](http://localhost:5001/)(if you use default port) at your browser. Now you ready to start coding your awesome project.

## Build

Install dependencies and type `NODE_ENV=production make build`. This rule is producing webpack build and regular golang build after that. Result you can find at `$GOPATH/bin`. Note that the binary will be named **as the current project directory**.

## Some issues

glide install失败 Update failed for golang.org/x/net: Cannot detect VCS

失败信息：

[WARN]    Unable to checkout golang.org/x/net
[ERROR]    Update failed for golang.org/x/net: Cannot detect VCS
[WARN]    Unable to checkout golang.org/x/sys
[ERROR]    Update failed for golang.org/x/sys: Cannot detect VCS
[WARN]    Unable to checkout golang.org/x/text
[ERROR]    Update failed for golang.org/x/text: Cannot detect VCS

以通过设置golang.org镜像到github.com/golang 来解决
```
1 $ glide mirror set https://golang.org/x/mobile https://github.com/golang/mobile --vcs git
2 $ glide mirror set https://golang.org/x/crypto https://github.com/golang/crypto --vcs git
3 $ glide mirror set https://golang.org/x/net https://github.com/golang/net --vcs git
4 $ glide mirror set https://golang.org/x/tools https://github.com/golang/tools --vcs git
5 $ glide mirror set https://golang.org/x/text https://github.com/golang/text --vcs git
6 $ glide mirror set https://golang.org/x/image https://github.com/golang/image --vcs git
7 $ glide mirror set https://golang.org/x/sys https://github.com/golang/sys --vcs git
```

## License
Apache License Version 2.0