> 为了防止误操作，移动端iOS操作系统针对原生click事件做了300ms的延迟，这在一定程度上影响了我们的使用体验。
 
#FastClick

现在有现成的插件fastclick可以解决这个问题，但是也有弊端：

- GitHub上最新版本的插件大小为25.4kb，轻量为趋势，能省则省。
- 它的核心思想是取消默认的click时间，判断当前dom节点的类型进行相应的操作，这个判断过程较为繁琐。

#MyTapEvent

本人最近在做微信项目，由于fastclick插件存在一定弊端，因此开发了一个简单的tap事件，主要思想有以下几点：

##Thinking

- 一次tap事件包含touchstart和touchmove(轻微移动)以及touchend三种状态
- callback方法在touchend后执行
- 根据chrome浏览器默认的判断取消点击的移动量，手指偏移量（水平或垂直）超过15px则判定为滚动，取消执行tap事件
- 手指按下时间过长不视为点击，默认时间间隔为500ms
- 使用HTMLElement来扩充原型，方便添加Event
- 使用单例模式，确保只加载一次


##Usage

```javascript
HTMLElement.addTapEvent(function(){
	//do something...
},isStopPropagation)

// isStopPropagation为是否阻止冒泡事件，默认为false

// 使用方法如：
document.querySelect('#test').addTapEvent(function(){
	alert('this is a tap event');
},true)

```


##Case

这里给一个移动端案例，同时也包含了闭包的知识，前20项为tap事件，后30项为click事件，大家可以在手机上试一下效果，感受一下两种方法的差别：

进入js目录运行命令：

```bash
node server.js
```
Server runing at port:3000

在浏览器中输入以下链接即可看到页面：
> localhost:3000/Demo.html

接着查看本机IP地址，Mac用户为ifconfig,Windows用户为ipconfig即可查看，然后将localhost替换即可，如：192.XXX.XX.X/Demo.html。接着就可以在手机中输入该地址即可感受一下效果差别啦（确保手机和电脑连接的同一个wifi）。
