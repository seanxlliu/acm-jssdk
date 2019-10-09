# AC Market JSSDK开发文档

## 概要

本文档描述了H5开发者如何集成AC Market SDK产品，通过集成AC Market SDK能够读取AC Market用户的信息，存储任意的JSON数据，以及展示广告创造收益。 

 - 用户信息，包括：用户名，地区，语言。
 - JSON数据：根据当前用户存储或者读取一个有限长度的JSON对象。
 - AC Market SDK目前提供了广告形式，包括：Banner(横幅广告)和激励视频。Banner 广告

## 集成前准备

### 获取账户信息

 - App Key: 开发者每个账号都有对应的App Key，请求广告时需要用到该参数
 - App Id: 开发者每创建一个应用后，系统会自动生成App Id
 - Custom Id: 用于区分不同的游戏或者广告位等，由开发者自定义, 可以传空字符串

### 初始化SDK
   ```javascript
   function init(args) {
      if (typeof shiningstars.acm.Manager != "undefined"){ // check the bridge 
         var manager = shiningstars.acm.Manager;
         if (manager && manager.version == SUPPORTED_VERSION) { // check if SDK version is supported
            // initialize API
            manager.initialize(APP_ID, APP_KEY, CUSTOM_ID);
            // check APIs
            var user = manager.User;
            var store = manager.Store;
            var event = manager.Event;
            var adloader = manager.AdLoader;
            var adBanner = manager.AdBanner;
         }
      }
   }
   ```

## AC Market SDK APIs

### Class shiningstars.acm.Manager

#### Properties

 - version: SDK version
 - User: 用户API
 - Store: 存储API
 - Event: 事件上报API
 - AdLoader: 激励广告API
 - AdBanner: 横幅广告API

#### Methods

 - addEventListener(callback): 注册侦听事件
 - exit(): 退出时调用

#### Events

 - PAUSE = 0: 当H5游戏所在的Activity被切换到后台时，触发Activity的onPause()事件
 - RESUME = 1: 当Activity切换到前台，触发Activity的onResume()事件

### Class shiningstars.acm.User

#### Properties

 - id: 用户ID
 - name: 用户名
 - location: 地区
 - lang: 语言

### Class shiningstars.acm.Store

#### Methods

 - setData(data, callback): 存储数据
 - getData(callback): 提取数据

### Class shiningstars.acm.Event

#### Methods

 - log(JSON.stringify(event), timestamp): 存储结构化事件, timestamp是UNIX timestamp, 如果是当前时间传0

### Class shiningstars.acm.AdLoader

#### Methods

 - addEventListener(callback): 注册侦听事件
 - load(): 用于提前加载广告，提前加载完成再播放利于提高用户体验
 - show(rewardId, userId): 展示广告

#### Events

 - AD_LOADED = 1: 加载完成可以使用show方法来播放
 - AD_COMPLETED = 2: 广告播放完毕
 - VIDEO_CLICKED = 3: 广告被点击
 - VIDEO_LOAD_FAILED = 4: 广告素材加载失败

#### Example

   ```javascript
   function callback(e) {
      if (e == adloader.AD_LOADED) {
         adloader.show(REWARD_ID, USER_ID);
      }
   }

   function show(args) {
      if (adloader){
         adloader.addEventListener(callback);
         adloader.load();
         // show some game information
      }
   }
   ```
### Class shiningstars.acm.AdBanner

#### Methods

 - show(): 显示底部Banner, 默认是显示状态
 - hide(): 隐藏Banner

#### Properties
 - height: Banner高度
 - width: Banner宽度

## 历史版本

### v0.1.2
 - 增加关闭接口
 - 增加Pause事件
 - 调整log参数顺序
  
### v0.1.1
 - 增加SDK版本查询
 - 更改API使用方式

### v0.1.0

 - 初始化文档
 - 用户信息
 - 数据存取
 - 广告加载