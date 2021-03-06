## 基本概念

为了避免恶意程序使用资源 URL 盗刷公网流量，给您带来不必要的损失，或使用恶意手法盗用本站资源，Bucket 提供防盗链配置功能，建议您通过控制台配置黑/白名单，通过对 HTTP 请求 Header 中 referer 字段的值进行约束，来进行安全防护。

## 配置说明

进入 COS 管理控制台，点击需要设置防盗链的 Bucket 上方 **基础配置** ：

![](https://mc.qcloudimg.com/static/img/7affe351956ede6c475d349f380f6a2b/image.png)

进行防盗链设置：

![](https://mc.qcloudimg.com/static/img/665340c9b36cd513f8a8f4f8a131b394/image.png)


功能状态：用户设置防盗链状态为开启后，必须填入相应的域名。

名单类型：可以设置名单中域名为黑名单或白名单。

> 黑名单：不允许名单中的域名出现在 HEADER 的 Referer 字段中，若两者匹配一致，则返回失败。
>
> 白名单：只允许名单中的域名出现在 HEADER 的 Referer 字段中，否则返回失败。

*注：如果通过 CDN 域名加速访问，则先行匹配 CDN 的防盗链规则，再匹配对象存储服务的防盗链规则。*

## 注意事项
名单支持多个域名且为前缀匹配，一个域名占一行，例如：

> 配置 www.qq.com ，则 Referer 为以下值时都匹配：
>
> `http://www.qq.com/123`
>
> `http://www.qq.com.cn`

支持带端口的域名和 IP，例如：

> abc.qq.com:8080
>
> 123.2.4.8:8080

支持通配符 * ，做二级域名或多级域名的通配，例如：

> 配置 *.qq.com，则 Referer 为以下值时都匹配：
>
> `http://a.b.qq.com/123`
>
> `http://a.qq.com`

若 Referer 为空（浏览器访问属于此种情况），默认不匹配任何规则：

> 用户设置了黑名单：空referer请求不会被黑名单规则拦截，COS正常返回该请求信息；
>
> 用户设置了白名单：空referer请求不会命中白名单规则，COS拒绝返回该请求信息。

*注：如果通过 CDN 域名加速访问，则先行匹配 CDN 的防盗链规则，再匹配对象存储服务的防盗链规则。*

## 示例

用户创建了一个 Bucket，并在根目录下放置了一张图片 1.jpg，根据规则生成了一个访问地址 `testbucket-1250000000.file.myqcloud.com/1.jpg`。

用户拥有网站 www.example.com，并将该图片嵌入了首页 index.html 中。

此时另一名持有 www.fake.com 的站长想把这张图片放入他的网站中，由于不想付流量费用，他便直接通过  `testbucket-1250000000.file.myqcloud.com/1.jpg` 地址引用了这张图片，并放置到首页 index.html 。

**开启前**

> 访问 `http://www.example.com/index.html` 图片显示正常。
>
> 访问 `http://www.fake.com/index.html` 图片也显示正常。

**开启方式一**

> 用户在 COS 配置了 Referer 为 **黑名单** 模式，填入了 *.fake.com 并保存生效。
>
> 访问 `http://www.example.com/index.html` 图片显示正常。
>
> 访问 `http://www.fake.com/index.html` 图片无法显示。

**开启方式二**

> 用户在 COS 配置了 Referer 为 **白名单** 模式，填入了 *.example.com 并保存生效。
>
> 访问` http://www.example.com/index.html` 图片显示正常。
>
> 访问 `http://www.fake.com/index.html` 图片无法显示。

