## 操作场景
本文为您介绍商业直播小程序插件解决方案的制定。

## 前提条件
- 注册 [腾讯云账号](https://cloud.tencent.com/document/product/378/17985)，并完成 [企业实名认证](https://cloud.tencent.com/document/product/378/10496)。
- 注册完成后提供腾讯云账号 APPID 以及 bizname。bziname 为您的唯一品牌标识，将会在域名前缀等被使用。bziname 一旦确定不可变更。
- 在 [微信公众平台](https://mp.weixin.qq.com) 注册并登录小程序。小程序类目请根据您的实际场景选择。
- 下载并安装最新版本的 [微信开发者工具](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html)。
- 使用小程序之前，请先阅读微信小程序提供的 [插件文档](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/)，了解插件的使用范围和限制。

## 操作步骤
### 推流及拉流
**1. 配置防盗链**
直播防盗链用于对推流端/播放端身份的权限鉴定，通过使用加密算法对推流 URL 或者播放 URL 进行加密，防止非法用户恶意盗推或者盗播。您可以提供推流密钥/播放密钥到腾讯云为您配置推流防盗链/播放防盗链。
默认情况下，为您自动开启推流防盗链配置。
**2. 生成推流/播放地址**
打开 https://bizlive.myqcloud.com/tools/address.html?bizname=bizname 工具页面( url 中的 bizname 参数为您的 bizname )，您可以在工具页面中填写流名称以及鉴权密钥，自动生成推流地址以及播放地址。

### 使用插件
6.1 申请插件使用权限
 - 在小程序管理后台的【设置】>【第三方设置】中选择【添加插件】，在弹出的面板中搜索【腾讯云直播助手】，选中插件并添加。
6.2 在小程序中引入插件代码，可以参考 [demo源码](https://bizlive-1258550678.cos.ap-chengdu.myqcloud.com/plugin-demo.zip)。
对于插件的使用者，使用插件前要在 app.json 中声明需要使用的插件，例如：
```js
   {
       ……
       "plugins": {
            "liveRoomPlugin": {
                "version": "1.0.3",
                "provider": "wx95a7d2b78cf30f98"
            }
        }
    }
```
6.3 使用插件中的推、拉流组件
- 在page的.json文件中定义需要引入的live-room-play组件，使用plugin://协议
```js
  {
  		"usingComponents": {
    			"live-room-play": "plugin://liveRoomPlugin/live-room-play"	//播放组件
  		}
  }
```
- 在 page 的 .wxml 文件加载上一步引入的 live-room-play 组件

```xml
  <view class="container-box">
    <view class="player-view">
      <live-room-play liveAppID="1258550678" playUrl="{{playUrl}}" orientation="{{orientation}}" objectFit="{{objectFit}}"
        minCache="{{minCache}}" maxCache="{{maxCache}}" mode="{{mode}}" muted="{{muted}}" debug="{{debug}}" bindPlayEvent="onPlayEvent" >
      </live-room-play>
    </view>
  </view>
```

直播播放相关的属性

<table width="850px">
  <tr align="center">
    <th width="80px">属性</th>
    <th width="80px">类型</th>
    <th width="80px">默认值</th>
    <th width="280px">说明</th>
  </tr>
  <tr align="center">
    <td>playUrl</td>
    <td>String</td>
    <td>""</td>
    <td>通过使用 https://bizlive.myqcloud.com/tools/address.html?bizname=bizname 工具页面获取</td>
  </tr>
  <tr align="center">
    <td>mode</td>
    <td>String</td>
    <td>"live"</td>
    <td>live（直播），RTC（实时通话，该模式时延更低）</td>
  </tr>
  <tr align="center">
    <td>orientation</td>
    <td>String</td>
    <td>"vertical"</td>
    <td>画面方向，可选值有 vertical，horizontal</td>
  </tr>
  <tr align="center">
    <td>objectFit</td>
    <td>String</td>
    <td>"contain"</td>
    <td>填充模式，可选值有 contain，fillCrop</td>
  </tr>  <tr align="center">
    <td>minCache</td>
    <td>Number</td>
    <td>1</td>
    <td>最小缓冲区，单位s</td>
  </tr>  <tr align="center">
    <td>maxCache</td>
    <td>Number</td>
    <td>3</td>
    <td>最大缓冲区，单位s</td>
  </tr>  <tr align="center">
    <td>muted</td>
    <td>Boolean</td>
    <td>false</td>
    <td>是否静音</td>
  </tr>  <tr align="center">
    <td>debug</td>
    <td>Boolean</td>
    <td>false</td>
    <td>是否显示日志</td>
  </tr>  <tr align="center">
    <td>bindPlayEvent</td>
    <td>EventHandle</td>
	<td></td>
    <td>播放状态变化事件回调</td>
  </tr>
</table>



6.4 在播放区域叠加额外展示信息
	- 组件提供了一个`<slot>`节点，用于承载组件引用时提供的子节点。本功能受限于微信，只能在组件上叠加 cover-image 、 cover-view 和 canvas。

### 7. 获取 live-room-play 组件实例
   7.1 为什么要获取 live-room-play 组件实例

   >  live-room 组件支持播放、停止播放、全屏等接口，要调用这些接口需要先获取到live-room-play的实例。
   >  7.2 怎么获取 live-room-play 组件实例
   >  live-room-play 是腾讯视频云直播插件中的一个组件，在腾讯视频云直播插件中暴露了获取 live-room-play 组件实例的接口，您只需要先在 page 的 .js 文件中，将插件加载进来，即可获取到 live-room-pla y组件实例

   ```js
   // 加载插件
   var plugin = requirePlugin("liveRoomPlugin")
   // 获取live-room组件实例
   var liveRoomComponent = plugin.instance.getLiveRoomInstance();
   ```

### 8.  live-room-play 接口
     live-room-play 组件提供如下接口

8.1 start
	>开始播放。调用之后会启动播放。在开始播放之前，除了5.3提到的key信息，`playUrl`也要保证已经设置到组件属性中。

```
​```js
// 获取live-room-play组件实例
var liveRoomComponent = plugin.instance.getLiveRoomInstance();
liveRoomComponent.start();
```
```

8.2 stop
	>结束播放。
	```js
	// 获取live-room-play组件实例
	var liveRoomComponent = plugin.instance.getLiveRoomInstance();
	liveRoomComponent.stop();
```

8.3 requestFullScreen
	>全屏播放。
	```js
	// 获取live-room-play组件实例
	var liveRoomComponent = plugin.instance.getLiveRoomInstance();
	liveRoomComponent.requestFullScreen(true);		//全屏播放
	//liveRoomComponent.requestFullScreen(false);	//退出全屏
	```

8. 事件
   8.1 播放事件

> 播放事件分为：

> 1.普通的播放事件，tag是`playEvent`，`code`含义见[状态码](https://developers.weixin.qq.com/miniprogram/dev/component/live-player.html)

> 2.错误事件，tag是`error`。现在只有白名单校验出错时会抛出，`code`是-1，具体的错误原因在`detail`里给出
>
> <table width="850px">
>  <tr align="center">
>    <th width="80px">code</th>
>    <th width="80px">含义</th>
>    <th width="80px">具体错误信息</th>
>  </tr>
>  <tr align="center">
>    <td>-1</td>
>    <td>白名单校验出错</td>
>    <td>具体的错误原因在detail里给出</td>
>  </tr>
> </table>

