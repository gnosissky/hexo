前面把后台的验证机制梳理了一下，主要是Cookie、Session和Token的使用，[Django：Cookie设置及跨域问题处理](https://www.jb51.net/article/223395.htm)、[Django实：Cookie搭配Session使用](https://www.jb51.net/article/223398.htm)、[Django：基于Token的验证使用](https://www.jb51.net/article/223377.htm)，当然这并不表示Token是最优的，还是需要根据项目需求来选择的，也可以混合搭配使用。

![img](https://img.jbzj.com/file_images/article/202109/2021091811493354.png)

今天要做的事将后台发送过来的Token存储到客户端中，这里可以存Cookie、LocalStorage、SessionStorage等地方，Cookie前面已经介绍过了这里直接忽略，我们主要说说LocalStorage和SessionStorage。

![img](https://img.jbzj.com/file_images/article/202109/2021091811493455.jpg)



## 什么是LocalStorage

LocalStorage译为“本地存储器”，是HTML5中新增的一个存储对象，跟Cookie一样也是用来本地存储来的，但是解决了Cookie存储空间不足的问题(cookie每条存储空间为4k)，而localStorage浏览器一般支持5M，通常以键/值对形式的字符串进行存储。



## 什么是SessionStorage

SessionStorage译为“会话存储”，也是HTML5新增的一个存储对象， 用于本地临时存储同一窗口的数据，在关闭窗口之后将会删除这些数据，SessionStorage浏览器一般支持5M，通常以键/值对形式的字符串进行存储。



## LocalStorage与SessionStorage的区别

LocalStorage生命周期是永久,除非主动清除LocalStorage信息，否则这些信息将一直存放在客户端。而SessionStorage生命周期是临时，仅在当前会话窗口有效，关闭页面或浏览器数据就会自动被清除。

![img](https://img.jbzj.com/file_images/article/202109/2021091811493456.jpg)



## LocalStorage与SessionStorage的特点

1.不同浏览器之间无法共享LocalStorage或SessionStorage中的数据。

2.LocalStorage和SessionStorage可以使用统一的API接口。

3.LocalStorage或SessionStorage通常以键/值对形式的字符串进行存储，所以在存储时需要对数据格式进行转换，使用JSON.stringify方法将对象转换成字符串，提取时用JSON.parse方法将字符串转换成对象。

4.LocalStorage或SessionStorage是HTML5的新属性，所以需要较新的浏览器才支持。

![https://img1.sycdn.imooc.com/5bab321d0001972206300152.jpg](https://img.jbzj.com/file_images/article/202109/2021091811493457.jpg)



## LocalStorage与SessionStorage的用法

因为LocalStorage与SessionStorage的应用一致，这里就不多做解释了，以LocalStorage为例。

LocalStorage提供了5个方法，分别是clear（清除LocalStorage）、getItem（获取本地数据）、key（取下标对应键的值）、removeItem（删除以保存数据）、setItem（设置保存数据）。

![img](https://img.jbzj.com/file_images/article/202109/2021091811493458.png)

 下面是具体的使用方法和说明，直接用localStorage.就可以带出来对应的方法，我们只要理解其对应的应用属性就可以使用了。

这样我们就可以使用localStorage.setItem('键','值')将服务器传过来的数据存入客户端本地，存储前记得将数据进行转换。

![img](https://img.jbzj.com/file_images/article/202109/2021091811493559.png)



## LocalStorage与SessionStorage应用实例

下面是我实际开发中的应用，前面我通过JsonResponse向前端发送了JSON数据，里面包含了data（用户请求的数据）、token（服务器生成的token）和code（后台处理的状态码），前端收到这些数据后对数据进行处理，判断code返回是否成功，如果成功我们就解析数据并将数据存入本地，否则则访问失败。

![img](https://img.jbzj.com/file_images/article/202109/2021091811493560.png)

这里我用 localStorage和sessionStorage分别存了两个数据，localStorage是自定义的，sessionStorage是从后台获取的，打开浏览器开发者工具，在Application中我们可以在Storage下面的localStorage和sessionStorage分别找到我们存储的对应值。

![img](https://img.jbzj.com/file_images/article/202109/2021091811493561.png)