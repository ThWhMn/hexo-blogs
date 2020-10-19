---
title: NaNNaNNaNNaN-Batman
tags:
  - Writeups
  - JS审计
  - WEB
categories: CTF
date: 2020-09-20 16:34:08
---

**膰聹聣eval莽颅聣暮聡藵膰聲掳暮呕拧膰聳聡膰聹殴莽藕聳莽聽聛莽職聞暮藵膮暮聯聧**

==这是被乱码毁掉的文章，就如同题目的考点一般==

<!-- more -->

盲赂聥膷藵藵茅聶聞盲钮艣膹藕聦膰聣聯暮藕聙暮聬聨

```html
<script>_='function $(){e=getEleById("c").value;length==16^be0f23233ace98aa$c7be9){tfls_aie}na_h0lnrg{e_0iit\'_ns=[t,n,r,i];for(o=0;o<13;++o){	[0]);.splice(0,1)}}}	\'<input id="c">< onclick=$()>Ok</>\');delete _var ","docu.)match(/"];/)!=null=["	write(s[o%4]buttonif(e.ment';for(Y in $='	')with(_.split($[Y]))_=join(pop());eval(_)</script>
```

盲拧膮莽艂聼莽艂聼膹藕聦暮聟艣盲赂颅膷偶聵膰聹聣盲拧膮莽聽聛膬聙聜

**方法一**

莽聰篓sublime text膰聣聯暮藕聙

<img src="NaNNaNNaNNaN-Batman/sublime.png" width=50%/>

暮聫聭莽聨掳膰聹聣暮陇搂茅聡聫膰聨搂暮聢艣暮颅聴莽殴艢膹藕聦暮陇聞莽聬聠膰聢聬暮聫呕膷搂聠膰聽藕暮藕聫

<img src="NaNNaNNaNNaN-Batman/visible.png" width=50%/>

暮掳聠 `eval(_);`膰聧藰膰聢聬 `console.log(_)`膹藕聦暮聧艂暮聫呕`F12`暮聬聨暮聹篓膰聨搂暮聢艣暮聫掳盲赂颅暮聫聭莽聨掳js盲钮艁莽聽聛

<img src="NaNNaNNaNNaN-Batman/console.png" width=50%/>

膰聢聳暮掳聠`eval(_);`膰聧藰膰聢聬 `alert(_)`暮聹篓暮藕拧莽艦聴盲赂颅膰聼慕莽聹聥

<img src="NaNNaNNaNNaN-Batman/alert.png" width=50%/>

**方法二**

莽聦聹膰木聥膰聵呕暮聣聧茅聺藰盲钮艁莽聽聛盲赂颅莽職聞盲赂聙盲艧聸暮颅聴莽殴艢膷藰扭`eval`膷沤膭莽沤聴盲艧聠膹藕聦膰聣聙盲钮慕盲拧膮莽聽聛膬聙聜

膰聲聟茅聡聡暮聫聳盲赂聤膷偶掳膰聸麓膰聰拧暮聡藵膰聲掳莽職聞膰聳拧膰艂聲

<img src="NaNNaNNaNNaN-Batman/sourcecode.png" width=50%/>

**方法一**

膷偶聸膷膭聦盲钮艁莽聽聛暮沤膭膷沤膭膹藕聦暮聫呕盲钮慕暮啪聴暮聡艧膷艢聛膰聻聞茅聙聽盲赂聙盲赂艦暮颅聴莽殴艢盲赂藳膹藕聦暮拧艣盲赂聰膰钮膭膷艣艂盲钮慕盲赂聥膰聺膭盲钮艣膹藕職

```
茅聲偶暮艧艢盲赂艧16
盲钮慕be0f23暮藕聙暮陇麓
盲钮慕e98aa莽钮聯暮掳啪
暮聦聟暮聬扭233ac
暮聦聟暮聬扭c7be9
暮聧艂 be0f233ac7be98aa
```

**方法二**

膷偶聬膷膭聦盲赂聥茅聺藰膷偶聶膰沤木js盲钮艁莽聽聛盲拧聼膷聝藵暮啪聴暮聢掳Flag

```js
var t = ["fl", "s_a", "i", "e}"];
        var n = ["a", "_h0l", "n"];
        var r = ["g{", "e", "_0"];
        var i = ["it'", "_", "n"];
        var s = [t, n, r, i];
        for (var o = 0; o < 13; ++o) {
            document.write(s[o % 4][0]);
            s[o % 4].splice(0, 1)
        }
// 盲赂聧盲藕職js
```

暮啪聴暮聢掳flag

<img src="NaNNaNNaNNaN-Batman/flag.png" width=50%/>