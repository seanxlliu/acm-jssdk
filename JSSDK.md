# AC Market JSSDK 开发文档

## 概要

本文档描述了 H5 开发者如何集成 AC Market SDK 产品，通过集成 AC Market SDK 能够读取 AC Market 用户的信息，存储任意的 JSON 数据，以及展示广告创造收益。

- 用户信息，包括：用户名，地区，语言。
- JSON 数据：根据当前用户存储或者读取一个有限长度的 JSON 对象。
- AC Market SDK 目前提供了广告形式，包括：Banner(横幅广告)和激励视频。Banner 广告

## 集成前准备

### 获取账户信息

- App Key: 开发者每个账号都有对应的 App Key，请求广告时需要用到该参数
- App Id: 开发者每创建一个应用后，系统会自动生成 App Id
- Custom Id: 用于区分不同的游戏或者广告位等，由开发者自定义, 可以传空字符串

### 初始化 SDK

```javascript
function managerEventCb(e) {
  // handle system events
}

function adCb(e) {
  // handle rewards
}

function init(args) {
  if (typeof shiningstars.acm.Manager != "undefined") {
    // check the bridge
    var manager = shiningstars.acm.Manager;
    if (manager && manager.version == SUPPORTED_VERSION) {
      // check if SDK version is supported
      // initialize API
      manager.initialize(APP_ID, APP_KEY, CUSTOM_ID);
      // check APIs
      var user = manager.User;
      var store = manager.Store;
      var event = manager.Event;
      var adloader = manager.AdLoader;
      var adBanner = manager.AdBanner;

      manager.addEventListener(managerEventCb);
      adloader.addEventListener(adCb);
    }
  }
}
```

## AC Market SDK APIs

### Class shiningstars.acm.Manager

#### Properties

- version: SDK version
- User: 用户 API
- Store: 存储 API
- Event: 事件上报 API
- AdLoader: 激励广告 API
- AdBanner: 横幅广告 API

#### Methods

- addEventListener(callback): 注册侦听事件
- exit(): 退出时调用

#### Events

- PAUSE = 0: 当 H5 游戏所在的 Activity 被切换到后台时，触发 Activity 的 onPause()事件
- RESUME = 1: 当 Activity 切换到前台，触发 Activity 的 onResume()事件

### Class shiningstars.acm.User

#### Properties

- id: 用户 ID
- name: 用户名
- location: 地区
- lang: 语言

### Class shiningstars.acm.Store

#### Methods

- setData(data, callback): 存储数据. callback(error, data), error - 错误消息, null 代表无错误。
- getData(callback): 提取数据. callback(error, data), error - 错误消息, null 代表无错误。 data 是之前按用户存储的数据。

### Class shiningstars.acm.Event

#### Methods

- log(JSON.stringify(event), timestamp): 存储结构化事件, timestamp 是 UNIX timestamp, 如果是当前时间传 0

### Class shiningstars.acm.AdLoader

#### Methods

- addEventListener(callback): 注册侦听事件
- load(): 用于提前加载广告，提前加载完成再播放利于提高用户体验
- show(rewardId, userId): 展示广告

#### Events

- AD_LOADED = 1: 加载完成可以使用 show 方法来播放
- AD_COMPLETED = 2: 广告播放完毕。可以利用该事件恢复游戏
- VIDEO_CLICKED = 3: 广告被点击, 这里注意有些广告并不要求点击, 因此在观看时间符合要求之后也会触发该事件。应该用这个事件来判断是否给予奖励
- VIDEO_LOAD_FAILED = 4: 广告素材加载失败

#### Example

```javascript
function adCb(e) {
  if (e == adloader.AD_LOADED) {
    adloader.show(REWARD_ID, USER_ID);
  }
}

function show(args) {
  if (adloader) {
    adloader.load();
    // show some game information
  }
}
```

### Class shiningstars.acm.AdBanner

#### Methods

- show(): 显示底部 Banner, 默认是显示状态
- hide(): 隐藏 Banner

#### Properties

- height: Banner 高度
- width: Banner 宽度

## 历史版本

### v0.1.2

- 增加关闭接口
- 增加 Pause 事件
- 调整 log 参数顺序
- 调整事件说明

### v0.1.1

- 增加 SDK 版本查询
- 更改 API 使用方式

### v0.1.0

- 初始化文档
- 用户信息
- 数据存取
- 广告加载
