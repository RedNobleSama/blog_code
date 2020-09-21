---
title: django-form使用和验证
toc: true
mathjx: true
cover: /2018/03/23/django-form使用和验证/head.png
tags:
  - Python
categories:
  - Python
  - Django
abbrlink: 10253
date: 2018-03-23 19:37:11
update:
---

### 创建Form
新建form.py

~~~Python
from django import forms


class LoginForm(forms.Form):
    username = forms.CharField(required=True,)
    password = forms.CharField(required=True, min_length=3)
~~~

规定form表单中字段的长度

### 使用form验证Post数据
~~~Python
class LoginView(View):
    def get(self, request):
        return render(request, 'user/login.html')

    def post(self, request, *args, **kwargs):
        form = LoginForm(request.POST)

        if form.is_valid():
            # 通过用户名和密码查询用户是否存在
            user_name = LoginForm.cleaned_data['username']
            password = LoginForm.cleaned_data['password']
            user = authenticate(username=user_name, password=password)
            if user is not None:
                login(request, user)
                return HttpResponseRedirect(reverse('login'))
        else:
            return render(request, 'user/login.html', {'login_form': form})
~~~
