<p align="center"><img src="http://ww1.sinaimg.cn/large/0065Zy9egy1fvcjfzaa1lj30dw0dwwhe.jpg" alt="eplayer logo" width="200px"></p>

# eplayer [![NPM version](https://img.shields.io/npm/v/eplayer.svg?style=flat-square)](https://npmjs.com/package/eplayer) [![NPM downloads](https://img.shields.io/npm/dm/eplayer.svg?style=flat-square)](https://npmjs.com/package/eplayer)

:dart: A web-components html5 video player facing future.

#### who use eplayer？

[epayer.js.org - 官网（demo）](https://eplayer.js.org/)

[clicli.us - C 站](https://www.clicli.us/)

### Use

1. 导入进来 js 文件，可以从 lib 目录获取

目前 web-components 原生支持度是在太差，尤其是国内各种奇葩浏览器（搜狗、QQ 等），需要引入 polyfill

```html
<script src="https://unpkg.com/@webcomponents/webcomponentsjs@2.2.1/webcomponents-bundle.js"></script>
<script src="./eplayer.js"></script>
```

2. 插入 `e-player` 标签，没错，只需要将平时用的 `video` 换成 `e-player` 即可

type 属性可选，默认为 mp4

```html
<e-player src="./001.mp4"></e-player>
```

3. 可选设置 css，用于穿透 shadow-dom 预留的默认样式

```css
e-player {
  /* 主题色 默认为哔哩哔哩同款蓝*/
  --theme: #f13e7b;
  /* 进度条底色 一般不用设置 */
  --progress: rgba(255, 255, 255, 0.3);
  /* 进度条偏移颜色 一般不用设置 */
  --buffer: rgba(255, 255, 255, 0.3);
  /* 图标颜色 一般不用设置 */
  --icons: rgba(255, 255, 255, 0.6);
}
```

#### hls & flv

原生支持 `mp4` 和 `mkv` ,如果需要支持 `m3u8` 和 `flv`，需要先引入 `hls.js` 和 `flv.js`

这两个文件太大，建议手动 gzip

```html
<script src="./hlv.min.js"></script>
<script src="./flv.min.js"></script>
```

然后，type 属性 对应 `hls` 或 `flv`

```html
<e-player src="./001.m3u8" type="hls"></e-player>
```

## Npm

```
npm i eplayer --save
```

注意，发布到 npm 的模块，只对外暴露了一个 Eplayer 类，需要自行 define customElements，并且是全局注册，只能一次

```Javascript
impport Eplayer from 'eplayer'

customElements.define('e-player',Eplayer)
```
同样的，polyfill 和 hls.js 等仍然以 cdn 的方式加入

这还没完，接着看：
#### Vue

vue 默认是不支持 web-components 的，它无法主动判断含有`-`的是 vue 组件还是 web 组件

所以需要手动配置，忽略掉`e-player`

```JavaScript
Vue.config.ignoredElements = [
  'e-player'
]
```

然后，同样需要引入上面的几个文件，然后 bind 一下 src 和 type

```html
<e-player :src="url" :type="type"></e-player>
```

可以封装成 vue 组件来使用：[vue-web-components-wrapper](https://github.com/vuejs/vue-web-component-wrapper)

#### React

react 直接支持 web-components，直接在 render 函数中`e-player`标签

同样的，JSX 并不把它当作 vnode tree 的孩子，需要手动操作 dom

通常为了方便的使用 ref，会封装成 react 组件来使用：

```Javascript
function Eplayer(props) {
  return <e-player src={props.src} type={props.type}></e-player>
}
```

#### ssr
ssr 服务端没有 web-components 的 API，通常 web-components 的 ssr 都会通过一些库去模拟这些 API
eplayer 不推荐这么做，请直接和正常的 html 引入方式一样，引入 cdn

#### 移动端

关于移动端，国产的手机浏览器太奇葩，没有好的兼容方法

目前会自动检测，移动端会自动替换为默认播放器，然后浏览器会自动替换 UI

#### Screenshot

![](https://ws1.sinaimg.cn/mw690/0065Zy9egy1fyrexxl5xkj30rq0flk1i.jpg)
