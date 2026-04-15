# THS H5 分享接入配方

## 1. 最小可用模板

适用于任意同花顺 H5 页面。

```html
<script>
  function showShareBtn() {
    window.callNativeHandler('notifyWebHandleEvent', {
      method: 'setTitleBar',
      params: {
        rightViewType: 'ShareImageView',
        stopSetNil: 'yes',
      },
    });
  }

  function buildSharePayload(data) {
    return JSON.stringify({
      type: 1,
      title: data.title,
      content: data.content,
      url: data.url || window.location.href,
      actionKey: data.actionKey || 'app_share',
      platforms: data.platforms || ['weixin', 'friends', 'qq', 'qqzone', 'douyin'],
      shareInfo: {
        needDefaultQR: false,
        mainImageUrl: '',
        logoImageUrl: '',
        QRCodeUrl: '',
      },
    });
  }

  function bindNativeShare(getShareData) {
    window.callNativeHandler('changeWebViewButton');
    window.registerWebHandler('changeWebViewButton', function () {
      var data = getShareData();
      window.callNativeHandler(
        'hexinShare',
        buildSharePayload(data),
        function () {
          showShareBtn();
        }
      );
    });
  }

  showShareBtn();

  bindNativeShare(function () {
    return {
      title: '页面标题',
      content: '页面摘要',
      url: window.location.href,
      actionKey: 'app_share_demo',
    };
  });
</script>
```

## 2. 项目里的可直接复用示例代码

如果用户要“按当前项目的方式接入”，优先给出下面这套代码，而不是只描述思路。

### 2.1 客户端右上角分享按钮

```ts
export const showShareBtn = () => {
  window.callNativeHandler('notifyWebHandleEvent', {
    method: 'setTitleBar',
    params: {
      rightViewType: 'ShareImageView',
      stopSetNil: 'yes',
    },
  })
}
```

### 2.2 右上角按钮点击后调用 `hexinShare`

```ts
window.callNativeHandler('changeWebViewButton')
window.registerWebHandler('changeWebViewButton', function () {
  const { contentKeyList, titleKeyList } = generateContentAndTitle(historyLatestItem)
  window.callNativeHandler(
    'hexinShare',
    JSON.stringify({
      type: 1,
      title: titleKeyList.join(' | '),
      content: contentKeyList.join(' | '),
      url: window.location.href,
      actionKey: 'app_history_anomaly_interpretation',
      platforms: ['weixin', 'friends', 'qq', 'qqzone', 'douyin'],
      shareInfo: {
        needDefaultQR: false,
        mainImageUrl: '',
        logoImageUrl: '',
        QRCodeUrl: '',
      },
    }),
    () => showShareBtn()
  )
})
```

### 2.3 微信二次分享

页面执行 `setWxTwiceShareInfo(pageTitle, pageDesc)` 即可支持微信二次分享。

```ts
export const appendScript = (src: string, onload: () => void) => {
  let script: HTMLScriptElement = document.createElement('script')
  script.src = src
  script.onload = onload
  document.head.appendChild(script)
}

export const setWxTwiceShareInfo = (pageTitle: string, pageDesc: string) => {
  const isInWeChat = navigator.userAgent.toLowerCase().indexOf('micromessenger') > -1
  if (isInWeChat) {
    const shareData = {
      link: window.location.href,
      title: pageTitle || '页面标题',
      desc: pageDesc || '页面描述',
      imgUrl: 'http://i.thsi.cn/sns/zixun/anomaly_share.png',
    }
    appendScript('//s.thsi.cn/cb?;js/m/v2.9/common/wxShare.js', () => {
      window.wxShare.init(shareData)
    })
  }
}
```

### 2.4 页面内最小接入方式

```ts
setTimeout(() => {
  showShareBtn()
}, 100)

bindNativeShare(() => ({
  title: pageTitle,
  content: pageDesc,
  url: window.location.href,
  actionKey: 'app_share_demo',
}))

setWxTwiceShareInfo(pageTitle, pageDesc)
```

## 3. 页面数据异步返回时

如果分享标题和摘要依赖接口数据，不要在数据未返回时拼文案。

```html
<script>
  var pageData = null;

  function getShareData() {
    if (!pageData) {
      return {
        title: '默认标题',
        content: '默认摘要',
        url: window.location.href,
        actionKey: 'app_share_demo',
      };
    }

    return {
      title: pageData.title,
      content: pageData.content,
      url: window.location.href,
      actionKey: 'app_share_demo',
    };
  }

  showShareBtn();
  bindNativeShare(getShareData);

  fetch('/api/detail')
    .then(function (res) { return res.json(); })
    .then(function (res) {
      pageData = res.data;
    });
</script>
```

如果页面的分享逻辑必须等数据 ready 后才可用，也可以在接口成功后再执行 `bindNativeShare(getShareData)`。

## 4. 抽成公共函数

推荐至少抽这三个函数：

```js
function showShareBtn() {}
function buildSharePayload(data) {}
function bindNativeShare(getShareData) {}
```

如果多个页面复用，再加一个：

```js
function initNativeShare(options) {
  showShareBtn();
  bindNativeShare(function () {
    return typeof options.getShareData === 'function'
      ? options.getShareData()
      : options;
  });
  if (options.pageTitle || options.pageDesc) {
    setWxTwiceShareInfo(options.pageTitle, options.pageDesc);
  }
}
```

如果需要微信二次分享，再补：

```js
function appendScript(src, onload) {}
function setWxTwiceShareInfo(pageTitle, pageDesc) {}
```

## 5. 文案生成模板

适用于标题和摘要需要统一拼接规则。

```js
function generateContentAndTitle(detail) {
  var titleParts = [];
  var contentParts = [];

  if (detail.name) titleParts.push(detail.name);
  if (detail.sceneTitle) titleParts.push(detail.sceneTitle);

  if (detail.date) contentParts.push(detail.date);
  if (detail.keywords && detail.keywords.length) {
    contentParts.push(detail.keywords.join('+'));
  }

  return {
    title: titleParts.join(' | '),
    content: contentParts.join(' | '),
    url: window.location.href,
    actionKey: detail.actionKey || 'app_share',
  };
}
```

## 6. 页面重新显示时补按钮

有些页面从后台恢复后，右上角按钮会丢。需要在页面 show 时再补一次。

如果宿主提供了 show 回调，就在回调里补：

```js
function onPageShow() {
  showShareBtn();
}
```

如果项目里有自己的页面显示事件总线，也是在 show 时机里只做这件事：

```js
showShareBtn();
```

## 7. 常用 payload 字段

```js
{
  type: 1,
  title: '分享标题',
  content: '分享摘要',
  url: window.location.href,
  actionKey: 'app_share_demo',
  platforms: ['weixin', 'friends', 'qq', 'qqzone', 'douyin'],
  shareInfo: {
    needDefaultQR: false,
    mainImageUrl: '',
    logoImageUrl: '',
    QRCodeUrl: '',
  }
}
```

说明：

- `type: 1` 通常表示普通链接分享
- `title` 和 `content` 由业务侧生成
- `url` 默认取当前页面地址
- `actionKey` 用于业务标识，按页面单独命名
- `mainImageUrl` 为空表示不传分享大图

微信二次分享常用字段：

```js
{
  link: window.location.href,
  title: pageTitle || '事件脉络',
  desc: pageDesc || '事件深度解读',
  imgUrl: 'http://i.thsi.cn/sns/zixun/anomaly_share.png'
}
```

## 8. 使用边界

适合：

- 同花顺客户端内的 H5 页面
- 右上角按钮由客户端标题栏承载
- 分享能力由 `window.callNativeHandler` 提供
- 微信内需要二次分享的页面

不适合：

- 普通浏览器网页
- 只想做 Web Share API
- 微信 JS-SDK 分享
- 不存在 `callNativeHandler` 的页面

## 9. 默认交付内容

当用户说“给这个 THS H5 页面加右上角分享”时，默认应交付：

1. `showShareBtn`
2. `buildSharePayload`
3. `bindNativeShare`
4. `appendScript`
5. `setWxTwiceShareInfo`
6. 页面内的初始化调用
7. 必要时的 on-show 补按钮逻辑
