[TOC]

#   krpano学习笔记

##  嵌入到HTML中（Embedding into HTML）

### 引入`krpano.js`

```javascript
    //`krpano.js`的名字是根据使用不同的生成全景的方式而不同的，比如: `pano.js`,`tour.js`
    <script src="krpano.js"></script>
```

### 嵌入全景图像

*   js调用`embedpano()`函数并传入相应的参数，将全景图显示在`id`为`pano`的div中，示例如下：

```html
    <div id="pano" style="width:100%;height:100%;"></div>
    <script>
        var config = {
            swf: "krpano.swf",
            pano: "pano.xml",
            target: "pano"
        }
		embedpano(config);
	</script>
```

*   `embedpano(config)`参数`config`是对象，其属性及属性值说明：
    +   xml: "pano.xml"
        -   xml所在路径相对于当前html路径引入
        -   如果不设置则会使用flash（`swf: krpano.swf`）播放全景
        -   启动时不想加载xml，可以将属性值设置为`null`
    +   target: "pano"
        -   将全景图嵌入到id为pano的div中
    +   swf："krpano.swf"
        -   基于当前flash的状况，此参数并非必须
        -   此参数主要作用是在html5不可用时，通过flash播放全景
    +   id: "krpanoSWFObject"
        -   此参数非必需，默认id为`krpanoSWFObject`
        -   主要作用是js操作时通过id获取krpano对象
        -   id值也可以自定义，当然在使用js获取krpano对象时需要使用自定义的id
    +   bgcolor: "#000000"
        -   此参数非必需
        -   全景图像显示区域的背景颜色，默认为`#000000`
    +   html5: "auto"
        -   此参数主要作用是设置全景图以HTML或flash的方式展示
        -   auto: 当HTML5播放器可用时使用HTML5，当不可用的时候使用flash播放器，前提是`xml`和`swf`属性均设置了正确的值
        -   prefer: 尽可能使用HTML播放器，其次使用flah播放
        -   fallback: 尽可能使用flash播放器，其次使用HTML5播放器
        -   only: 无论什么情况都只使用HTML播放器，当系统或浏览器不支持时会报错
        -   always: 只是用HTML5播放，此参数值不可用于上线系统，仅供内部测试
        -   never: 只是用flash播放
        -   扩展参数值
            -   +webgl: 仅当webgl可用时才会使用html5播放，注意，某些形状必需使用webgl
            -   +css3d: 优先使用css3d，此参数值仅供内部测试
        -   示例
            -   prefer+webgl: 当webgl可用时优先使用html5播放，否则使用flash
            -   only+webgl: 当webgl可用时只使用html5播放，否则报错
    +   flash: ""
        -   此参数设置全景通过flash播放，设置逻辑与html5的相反
        -   如果同时设置了flash和html5，则html5会被覆盖
        -   auto
        -   prefer
        -   fallback
        -   only
        -   never
    +   wmode: ""
        -   次参数配合flash使用，设置flash播放的一些参数，鉴于flash当前状况，次参数详细信息可以忽略
        -   [wmode](https://krpano.com/docu/html/#wmode): 详细参考官网
    +   localfallback: "http://localhost:8090"
        -   当使用`krpano Testing Server`启动全景播放时，此属性及属性值为默认值
        -   当使用flash播放时，会忽略次属性
    +   vars: {}
        -   `config`对象中传入一个配置对象
        -   此配置对象用来在xml文件加载后对其进行新的设置或覆盖之前的设置
        
        ```javascript
            var settings = {};
            settings["onstart"] = "trace('on start...')";
            settings["view.hlookat"] = 30;
            embedpano({xml:"pano.xml", target:"pano", vars:settings});
        ```

    +   [initvas](https://krpano.com/docu/html/#initvars): {}
        -   向`config`对象中传入一个配置对象，与`vars`属性类似，但是此对象是在xml加载完成前执行的
        -   次功能主要被用于在xml加载完成前进行url及占位符的设置
    +   basepath: ""
        -   设置用于解析`swf`等文件的路径
        -   设置在flash或html中引用xml文件的相对路径
    +   consolelog: false
        -   是否在浏览器控制台打印出krpano的log信息，默认值为false
    +   mwheel: true
        -   设置是否支持鼠标滚轮控制vr播放器的行为，比如缩放
        -   默认值为true，表示可以通过滚轮的滚动对全景图进行缩放
    +   capturetouch: true
        -   默认值为true，表示触摸事件被捕捉，并在vr播放器中使用
        -   默认值为false的时候，触摸事件仍然会被捕捉，但默认事件的处理不会停止
    +   focus: true
    +   webglsettings: {}
        -   向`config`对象中传递一个具有特殊设置的webgl上下文对象，因为webgl上下文在启动时创建，不能在运行时更改，所以这些设置必需在创建的过程中设定
        -   可用的设置：
            -   preserveDrawingBuffer： 默认为false
            -   depth
            -   stencil： 默认为false
            -   failIfMajorPerformanceCaveat： 默认为false 
    +   mobilescale: 0.5
        -   默认在移动端krpano会缩放0.5
        -   如果禁用默认则需要将`mobilescale`属性值设置为1
    +   fakedevice: ""
        -   模拟krpano在不同的设备（mobile，tablet，desktop）中播放
    +   onready: function
        -   krpano渲染完毕后执行的回调函数

        ```javascript
            embedpano({xml:"tour.xml", target:"pano", html5:"only", mobilescale:1.0, passQueryParameters:true, onready: showLog});

            function showLog(krpano) {
                krpano.call("showlog");
            }
        ```
    
    +   onerror: function
        -   krpano渲染出错时调用的回调函数，使用方法参考`onready`
    

### 

##  静态XML（XML Reference）

##  动态XML（Actions / Scripting）

##  HTML5

##  插件（plugins）

##  Javascript API