## demo介绍
该demo是为了学习iframe属性

其中sandbox特性是比较重要的。
## 项目启动

> `node index.js`

### iframe的特性

1. allow 可以为iframe指定特性策略

特性策略可以允许你控制**页面**或者**iframe**可以使用哪些特性。
**页面**控制的话设置在HTTP头部的Feature-Policy的这个字段，iframe的话就是我们要说的这个allow字段。

书写规则：` <feature name> <allowlist of origin(s)>`

allowlist有以下属性:
- *：表示该特性在该文档下都是允许的，包括所有的嵌套的浏览内容(iframes),而不用管这些内容的源。
- self：表示该特性在该文档下都是允许的，并且仅在同源的情况下嵌套的浏览内容(iframes)才可以使用。
- src：(iframes的allow属性专用)表示该特性在这个iframe下允许使用，只要加载的文档来源的源和iframe的src属性指定的URL是同源的。
- none：表示该特性在顶层以及嵌套的浏览内容下都是被禁用的<origin(s)>：表示该特性只在一些指定的源下才允许使用，多个源使用空格隔开

举些例子：
`<iframe allow="camera 'none'; fullscreen 'none'">` 不允许摄像机、全屏

`<iframe src="https://example.com..." allow="fullscreen 'src'"></iframe>` 只允许同源才可以使用全屏这个特性

`<iframe src="https://google-developers.appspot.com/demos/..."allow="geolocation https://google-developers.appspot.com"></iframe>` 只允许指定源才可以使用定位功能


2. allowfullscreen
是否允许iframe调用requestFullscreen()方法激活全屏模式，这个属性等同于allow属性的这个配置：allow="fullscreen"

3. allowpaymentrequest
是否允许一个跨域的iframe调用支付请求API

4. csp
内嵌的资源强制实行同源策略

5. height
iframe的高度，默认150px

6. importance
标识在iframe属性src指示的资源的下载优先级，有auto/high/low三个等级

7. name
内嵌的浏览内容的目标名称

8. referrerpolicy

这个属性牵扯到了HTTP的referer策略，我们知道referer的策略是这样的：

- No Referrer：任何情况下都不发送 Referrer 信息；
- No Referrer When Downgrade：仅当发生协议降级（如 HTTPS 页面引入 HTTP 资源，从 HTTPS 页面跳到 HTTP 等）时不发送 Referrer 信息。这个规则是现在大部分浏览器默认所采用的；
- Origin Only：发送只包含 host 部分的 Referrer。启用这个规则，无论是否发生协议降级，无论是本站链接还是站外链接，都会发送 Referrer 信息，但是只包含协议 + host 部分（不包含具体的路径及参数等信息）；
- Origin When Cross-origin：仅在发生跨域访问时发送只包含 host 的 Referrer，同域下还是完整的。它与 Origin Only 的区别是多判断了是否 Cross-origin。需要注意的是协议、域名和端口都一致，才会被浏览器认为是同域；
Unsafe URL：无论是否发生协议降级，无论是本站链接还是站外链接，统统都发送 Referrer 信息。正如其名，这是最宽松而最不安全的策略；


9. sandbox(可以取多个值)

`<iframe sandbox=""></iframe>`开启沙箱模式后，iframe内部就不允许使用脚本了。

- allow-forms 允许资源提交表单
- allow-modals 允许资源打开模态窗口，比如window.alert()、window.confirm()、window.print()、window.prompt()
- allow-scripts
- allow-same-origin	允许将内容作为**普通来源**对待。如果未使用该关键字，嵌入的内容将被视为一个独立的源。


当被嵌入的文档与主页面同源时，强烈建议不要同时使用**allow-scripts和allow-same-origin**，否则的话将允许嵌入的文档通过代码删除sandbox属性。虽然你可以这么做，但是这样的话其安全性还不如不用sandbox。

如果攻击者可以将潜在的恶意内容往用户的已沙箱化的iframe中显示，那么沙箱操作的安全性将不再可靠。推荐把这种内容放置到独立的专用域中，减小可能的损失。

沙箱属性(sandbox)在Internet Explorer 9及更早的版本上不被支持


### 参考文档

[原文](https://juejin.cn/post/6844903788969459719)
[Feature-Policy](https://github.com/w3c/webappsec-permissions-policy/blob/main/features.md)