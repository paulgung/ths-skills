---
name: ths-h5-native-share
description: 为同花顺 H5 页面接入客户端右上角分享功能。适用于用户要求“给 THS H5 页面加右上角分享”“接入 setTitleBar、changeWebViewButton、hexinShare”“复用同花顺客户端协议做分享”时。输出基于 window.callNativeHandler 和 window.registerWebHandler 的最小可用实现。
---

# ths-h5-native-share

用于给同花顺 H5 页面接入客户端右上角分享，并按需补充微信二次分享，不是通用浏览器分享 skill。

目标链路固定为：

1. 调用 `notifyWebHandleEvent.setTitleBar` 显示右上角分享按钮
2. 调用 `changeWebViewButton` 并注册 `registerWebHandler`
3. 用户点击右上角分享按钮后，调用 `hexinShare`
4. 分享完成后再次调用 `showShareBtn()`，避免按钮状态丢失
5. 如果页面运行在微信内，调用 `setWxTwiceShareInfo()` 配置微信二次分享

## 何时使用

当用户提到下面任一类需求时使用本 skill：

- 同花顺 H5 页面右上角加分享
- 调客户端协议实现分享
- 接 `window.callNativeHandler`
- 接 `hexinShare`
- 接 `changeWebViewButton`
- 复用 THS 客户端分享按钮

如果用户只是要普通浏览器分享、微信 H5 分享、Web Share API，不要用这个 skill。

## 工作流

1. 确认页面运行在同花顺客户端 WebView。
2. 确认分享数据来源：
   页面初始化就有，还是接口返回后才有。
3. 打开 [references/ths-h5-share-recipes.md](references/ths-h5-share-recipes.md)，直接套最小模板或项目示例代码。
4. 把协议调用收敛成公共函数：
   `showShareBtn`、`buildSharePayload`、`bindNativeShare`、`appendScript`、`setWxTwiceShareInfo`
5. 如果页面会被切到后台再回来，补一次 on-show 时的 `showShareBtn()`。
6. 完成后检查：
   - 右上角分享按钮是否出现
   - 点击后是否能拉起客户端分享面板
   - 标题、摘要、URL 是否正确
   - 分享后右上角按钮是否还在
   - 微信内打开时二次分享标题、摘要、图片是否正确

## 默认实现

优先输出下面三段能力：

1. `showShareBtn()`
2. `bindNativeShare(getShareData)`
3. `setWxTwiceShareInfo(pageTitle, pageSubTitle)`
4. 页面数据 ready 后调用一次绑定逻辑

除非用户明确要求，否则不要默认加入：

- 分享海报生成
- 端外浏览器分享降级
- 业务埋点

## 实施原则

- `setTitleBar` 只负责显示按钮，不负责发起分享
- `changeWebViewButton` 负责监听客户端按钮点击
- `hexinShare` 负责真正发起分享
- `setWxTwiceShareInfo` 负责微信环境下的二次分享配置
- 分享内容 builder 单独封装，不要写死在回调内部
- 如果分享文案依赖异步接口，等数据 ready 后再绑定或更新
- 如果页面反复进入，注意避免重复注册同一个 handler
- skill 必须给出可直接复制的项目示例代码，不能只写原则描述

## 输出要求

完成接入后，给出：

- 页面里新增的分享入口函数
- `title`、`content`、`url` 的来源
- 微信二次分享的 `title`、`desc`、`imgUrl` 来源
- 是否包含 on-show 重新展示按钮逻辑
- 仍需客户端验证的部分
