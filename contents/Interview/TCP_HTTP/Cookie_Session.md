## Cookie
### 什么是cookie？
http是一个无状态协议，什么是无状态呢？就是说这一次请求和上一次请求是没有任何关系的，互不认识的，没有关联的。这种无状态的的好处是快速。坏处是假如我们想要把www.zhihu.com/login.html和www.zhihu.com/index.html关联起来，必须使用某些手段和工具。

由于http的无状态性，为了使某个域名下的所有网页能够共享某些数据，session和cookie出现了。客户端访问服务器的流程如下：
* 首先，客户端会发送一个http请求到服务器端。
* 服务器端接受客户端请求后，建立一个session，并发送一个http响应到客户端，这个响应头，其中就包含Set-Cookie头部。该头部包含了sessionId。Set-Cookie格式如下，具体请看Cookie详解
Set-Cookie: value[; expires=date][; domain=domain][; path=path][; secure]
* 在客户端发起的第二次请求，假如服务器给了set-Cookie，浏览器会自动在请求头中添加cookie
* 服务器接收请求，分解cookie，验证信息，核对成功后返回response给客户端

### cookie 常用属性项
| 属性项 | 属性项介绍 |
| --- | --- |
| Name	| 键值对，可以设置要保存的 Key/Value，注意这里的 NAME 不能和其他属性项的名字一样|
| Expires |	过期时间，在设置的某个时间点后该 Cookie 就会失效 |
| Domain	| 生成该 Cookie 的域名，如 domain="www.baidu.com" |
| Path |	该 Cookie 是在当前的哪个路径下生成的，如 path=/wp-admin/ |
| httpOnly | 这个选项用来设置cookie是否能通过js去访问。默认情况下，cookie不会带httpOnly选项(即为空)|
|Secure |	如果设置了这个属性，那么只会在 SSH 连接时才会回传该 Cookie |

### Expires
该属性用来设置Cookie的有效期。Cookie中的maxAge用来表示该属性，单位为秒。Cookie中通过getMaxAge()和setMaxAge(int maxAge)来读写该属性。maxAge有3种值，分别为正数，负数和0。

如果maxAge属性为正数，则表示该Cookie会在maxAge秒之后自动失效。浏览器会将maxAge为正数的Cookie持久化，即写到对应的Cookie文件中（每个浏览器存储的位置不一致）。无论客户关闭了浏览器还是电脑，只要还在maxAge秒之前，登录网站时该Cookie仍然有效。下面代码中的Cookie信息将永远有效。

当maxAge属性为负数，则表示该Cookie只是一个临时Cookie，不会被持久化，仅在本浏览器窗口或者本窗口打开的子窗口中有效，关闭浏览器后该Cookie立即失效。

当maxAge为0时，表示立即删除Cookie。

### Domain
Cookie是不可以跨域名的，隐私安全机制禁止网站非法获取其他网站的Cookie。

正常情况下，同一个一级域名下的两个二级域名也不能交互使用Cookie，比如book.jd.com和shouji.jd.com，因为二者的域名不完全相同。如果想要jd.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数为.jd.com，这样使用xinfang.jd.com和diannao.jd.com 就能访问同一个cookie了。

### Path
path属性决定允许访问Cookie的路径。比如，设置为"/"表示允许所有路径都可以使用Cookie。

### httpOnly
这个选项用来设置cookie是否能通过 js 去访问。默认情况下，cookie不会带httpOnly选项(即为空)，所以默认情况下，客户端是可以通过js代码去访问（包括读取、修改、删除等）这个cookie的。当cookie带httpOnly选项时，客户端则无法通过js代码去访问（包括读取、修改、删除等）这个cookie。

在客户端是不能通过js代码去设置一个httpOnly类型的cookie的，这种类型的cookie只能通过服务端来设置。

## Session


## cookie与session的区别
主要有以下几点：
* 存在的位置：cookie以文本格式存储在浏览器上，存储量有限；而session存储在服务端；
* 安全性：cookie是以明文的方式存放在客户端的，安全性低，可以通过一个加密算法进行加密后存放；  session存放于服务器的内存中，所以安全性好
* 网络传输限制：cookie会传递消息给服务器；  session本身存放于服务器，不会有传送流量；
* 生命周期：cookie的生命周期是累计的，从创建时，就开始计时，20分钟后，cookie生命周期结束；session的生命周期是间隔的，从创建时，开始计时如在20分钟，没有访问session，那么session生命周期被销毁。

