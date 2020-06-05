---
title: Django Template Language语法
toc: true
mathjx: true
cover: /2018/05/017/Django Template Language语法/head.png
date: 2018-05-17 14:52:37
update:
tags: [Django]
categories: Django
---
## 模板继承
base.html文件，比如上方的导航栏

~~~
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="style.css">
    <title>{% block title %}My amazing site{% endblock %}</title>
</head>
<body>
    <div id="sidebar">
        {% block sidebar %}
        <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/blog/">Blog</a></li>
        </ul>
        {% endblock %}
    </div>
    <div id="content">
        {% block content %}{% endblock %}
    </div>
</body>
</html>
~~~
其它功能模块的html文件

~~~
{% extends "base.html" %}

{% block title %}My amazing blog{% endblock %}

{% block content %}
{% for entry in blog_entries %}
    <h2>{{ entry.title }}</h2>
    <p>{{ entry.body }}</p>
{% endfor %}
{% endblock %}
~~~

## 变量
~~~
{{ variable }}
~~~
.或获取变量的属性

循环字典
~~~
{% for k, v in defaultdict.items %}
    Do something with k and v here...
{% endfor %}
~~~

## 过滤器
~~~
{{ value|add:"2" }} 字符串或列表相加，注意：字符串是变成整数相加
{{ first|add:second }}
{{ value|addslashes }} 单引号前面加上/ 比如把 I’m using Django 变成 I\'m using Django
{{ value|capfirst }} 首字母大写
{{ value|center:"15" }} 15个占位符居中对齐
{{ value|ljust:"10" }} 10个占位符左对齐
{{ value|rjust:"10" }} 10个占位符右对齐
{{ value|cut:" " }} 移除掉所有的空格，如要移除所有的A, {{ value|cut: “A” }}
{{ value|date:"D d M Y" }} 参考
{{ value|default:"nothing" }} 如果value是False的话，使用默认值
{{ value|default_if_none:"nothing" }} 如果value是None的话，使用默认值
{{ value|dictsort:"name" }} 对列表中的多个字典进行排序
{{ value|divisibleby:"3" }} value能否被3整除，返回True或者False
{{ value|filesizeformat }} 文件大小变成人类直观可读的方式，value是123456789，变成117.7MB
{{ value|first }} 返回列表的第一个元素或字符串的第一个字符
{{ value|last }} 返回列表的最后一个元素回字符串的最后一个字符
floatformat 各种格式化
force_escape 再关闭转义的区域，强制启用转义
{{ value|get_digit:"2" }} 从右边往左取第2为，如果是123456789？
{{ value|join:" // " }} 使用字符串连接列表 [‘a’, ‘b’, ‘c’]变成了 “a // b // c”
{{ value|make_list }} 将数字或字符串循环到列表
{{ value|random }} 字符串或列表中随机获取一个
{{ value|safe }} 对value关闭自动转义
{{ some_list|safeseq|join:", " }} 对列表中每一个对象关闭自动转义，然后字符串拼接
{{ value|slice:":2" }} 获取列表的前两个元素，或字符串的前2为
{{ value|slugify }} 支持英文的slug
{{ value|stringformat:"E" }} 科学计数法表示
{{ value|striptags }} 过滤掉html中的标签
{{ blog_date|timesince:comment_date }} 人性化的时间显示
{{ value|title }} 标题大写
{{ value|truncatechars:9 }} 截取字符串，超过6后面会变成 ...
{{ value|truncatechars_html:9 }} 截取html标签中的字符串，超过6后面变成...
{{ value|truncatewords:2 }} 截取字单词，超过6后面会变成 ...
{{ value|truncatewords_html:2 }} 截取html标签中的单词，超过6后面变成...
{{ value|upper }} 字母变大写
{{ name|lower }} 字母变小写
{{ value|urlencode }} url编码
把https://www.example.org/变成https%3A%2F%2Fwww.example.org%2F
{{ value|urlizetrunc:15 }}
把Check out www.djangoproject.com 变成Check out <a href="http://www.djangoproject.com" rel="nofollow">www.djangopr...</a>
{{ value|wordcount }} 统计单词个数
{{ value|wordwrap:5 }} 每一行限定多少个，多了就换行
~~~
## 标签
for循环标签
~~~
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>
~~~

以及for … empty
~~~
{% thumbnail article.image "1920x1080" as im %}
    <img src="{{ im.url }}" alt="文章图片" class="card-img-top">
{% empty %}
    <img class="card-img-top" src="http://placehold.it/1920x1080" alt="图片大小">
{% endthumbnail %}
~~~

if, elif, and else
~~~
{% if athlete_list %}
    Number of athletes: {{ athlete_list|length }}
{% elif athlete_in_locker_room_list %}
    Athletes should be out of the locker room soon!
{% else %}
    No athletes.
{% endif %}
~~~

加上条件判断 ==, !=, <, >, <=, >=, in, not in, is, 以及 is not
~~~
{% if athlete_list and coach_list or cheerleader_list %}
{% if somevar == "x" %}
  This appears if variable somevar equals the string "x"
{% endif %}
~~~

cycle 循环常量或变量
~~~
{% for o in some_list %}
    <tr class="{% cycle 'row1' rowvalue2 'row3' %}">
        ...
    </tr>
{% endfor %}
~~~

filter过滤器，使用单个或多个过滤器
~~~
{% filter force_escape|lower %}
    This text will be HTML-escaped, and will appear in all lowercase.
{% endfilter %}
~~~

firstof 输出第一个为True的值
~~~
{% firstof var1 var2 var3 %}
~~~

extends继承父模板
~~~
{% extends "./base2.html" %}
{% extends "../base1.html" %}
{% extends "./my/base3.html" %}
~~~
now 展示当前日期或时间
~~~
{% now "jS F Y H:i" %}
~~~

## HTML转义
安全起见，默认开启自动转义；有如下2种方式关闭

对于变量
~~~
{{ article.get_markdown|safe }}
~~~
对于block(区域)
~~~
Auto-escaping is on by default. Hello {{ name }}

{% autoescape off %}
    This will not be auto-escaped: {{ data }}.

    Nor this: {{ other_data }}
    {% autoescape on %}
        Auto-escaping applies again: {{ name }}
    {% endautoescape %}
{% endautoescape %}
~~~
在已关闭转义的区域中启用转义
~~~
{% autoescape off %}
    {{ title|escape }}
{% endautoescape %}
~~~

## 方法调用
比如QuerySet中的方法
~~~
{% for tag in article.tags.all %}
    <a href="#"><span class="badge badge-info">{{ tag }}</span></a>
{% endfor %}
~~~
