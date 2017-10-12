## Universal Links  打通深度链接
#### JS唤醒本地app常规通用做法

* H5需要区分是否微信，qq浏览器等腾讯系软件。区分iOS系统版本，区分安卓系统版本。
iOS系统版本 

* 如果iOS系统版本 < 9.0系统版本   —>     URL Scheme    —>   比如Safari尝试直接打开app和加载定时器，超过时间，如果打不开，则定时器跳转至商店下载
如果iOS系统版本 > 9.0系统版本 —>     Universal Links直接打开（无视腾讯软件）

* 备注：因为微信是禁止iOS端的深度链接得，所以需要先判断是否是微信、QQ浏览器，如果是,需要提示用户用Safari打开（加一个中间引导页或者应用宝之类的），然后到达Safari。通过定时器判断，是否安装了app如果安装就唤醒，没有安装就跳到下载地址。

#### iOS9开启Universal Links  

打通深度链接（H5打开本地APP）

具体操作：

* 首先，你必须有一个域名，且这个域名的网站需要支持https，然后拥有网站的上传到.well-known目录的权限（这个权限是为了上传一个Apple指定的文件apple-app-site-association）
* 当你点击这个链接,应该是下载apple-app-site-association文件.
* []() https://yohunl.com/.well-known/apple-app-site-association


