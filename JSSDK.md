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
 - Custom Id: 用于区分不同的游戏或者广告位等，由开发者自定义

## 集成SDK

 - SDK加载加入页面的body部分 
    ```html 
    <script type="text/javascript" src="//cdn.directapk.net:8085/shiningstar/acm/sdk.js"></script>
    ```

 - 初始化Manager
    ```javascript
    var manager = new shiningstar.acm.Manager(APP_ID, APP_KEY, CUSTOM_ID);
    ```

## AC Market SDK APIs

### Class shiningstar.acm.User

#### Methods
 - User(manager): manager 是Manager class创建的实例

#### Properties

 - id: 用户ID
 - name: 用户名
 - location: 地区
 - lang: 语言

### Class shiningstar.acm.Store

#### Methods

 - Store(manager): manager 是Manager class创建的实例
 - setData(json): 存储数据
 - getData(): 提取数据

### Class shiningstar.acm.AdLoader

#### Methods

 - AdLoader(manager): manager 是Manager class创建的实例
 - addEventListener(event_id, callback): 注册侦听事件
 - load(): 用于提前加载广告，提前加载完成再播放利于提高用户体验
 - show(reward_id, user_id): 展示广告

#### Events
 - AD_LOADED: 加载完成可以使用show方法来播放
 - AD_COMPLETED: 广告播放完毕
 - VIDEO_CLICKED: 广告被点击
 - VIDEO_LOAD_FAILED: 广告素材加载失败

## 历史版本

### v0.1.0

 - 初始化文档
 - 用户信息
 - 数据存取
 - 广告加载