#针对高级程序员的前言

## Flask 中的本地线程对象

Flask 的设计原则之一是简单的任务不应当使用很多代码，应当可以简单地完成，但同时 又不应当把程序员限制得太死。因此，一些 Flask 的设计思路可能会让某些人觉得吃惊， 或者不可思议。例如， Flask 内部使用本地线程对象，这样就可以不用为了线程安全的 缘故在同一个请求中在函数之间传递对象。这种实现方法是非常便利的，但是当用于附属 注入或者当尝试重用使用与请求挂钩的值的代码时，需要一个合法的环境。 Flask 项目 对于本地线程是直言不讳的，没有一点隐藏的意思，并且在使用本地线程时在代码中进行 了标注和说明。

## 做网络开发时要谨慎

做网络应用开发时，安全要永记在心。

如果你开发了一个网络应用，那么可能会让用户注册并把他们的数据保存在服务器上。 用户把数据托付给了你。哪怕你的应用只是给自己用的，你也会希望数据完好无损。

不幸的是，网络应用的安全性是千疮百孔的，可以攻击的方法太多了。 Flask 可以防御 现代 Web 应用最常见的安全攻击：跨站代码攻击（ XSS ）。 Flask 和 下层的 Jinja2 模板引擎会保护你免受这种攻击，除非故意把不安全的 HTML 代码放进来。但是安全攻击 的方法依然还有很多。

这里警示你：在 web 开发过程中要时刻注意安全问题。一些安全问题远比想象的要复杂 得多。我们有时会低估程序的弱点，直到被一个聪明人利用这个弱点来攻击我们的程序。 不要以为你的应用不重要，还不足以别人来攻击。没准是自动化机器人用垃圾邮件或恶意 软件链接等东西来填满你宝贵的数据库。

Flask 与其他框架相同，你在开发时必须小心谨慎。

## Python 3 的情况

目前， Python 社区正处在改进库的过程中，以便于加强对 Python 语言的新迭代的 支持。虽然现在情况已经有很大改善，但是还是存在一些问题使用户难以下决心现在就 转向 Python 3 。部分原因是 Python 语言中的变动长时间未经审核，还有部分原因是 我们还没有想好底层 API 针对 Python 3 中 unicode 处理方式的变化应该如何改动。

我们强烈建议你在开发过程中使用 Python 2.6 或者 Python 2.7 ，同时打开 Python 3 警告。如果你计划在近期升级到 Python 3 ，那么强烈推荐阅读[如何编写向前兼容的 Python 代码](http://lucumr.pocoo.org/2011/1/22/forwards-compatible-python/) 。

如果你一定要使用 Python 3 ，那么请先阅读 [Python 3 支持](http://dormousehole.readthedocs.org/en/latest/python3.html#python3-support) 。


*© Copyright 2013, Armin Ronacher. Created using [Sphinx](http://sphinx.pocoo.org/).*