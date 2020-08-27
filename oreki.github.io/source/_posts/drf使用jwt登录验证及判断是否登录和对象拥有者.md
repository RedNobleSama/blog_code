---
title: drf使用jwt登录验证及判断是否登录和对象拥有者
toc: true
mathjx: true
cover: /2019/03/17/drf使用jwt登录验证及判断是否登录和对象拥有者/head.png
date: 2019-03-17 23:12:29
update:
tags: ['Django']
categories:
  - Python
  - Django
  - restframework
---

### 安装
> pip install djangorestframework-jwt

### setting.py中配置
~~~Python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
            'rest_framework.authentication.BasicAuthentication',
            'rest_framework.authentication.SessionAuthentication',
        ),
}
~~~


### urls.py中配置
~~~Python
from rest_framework_jwt.views import obtain_jwt_token
urlpatterns = [
    # url(r'^api-token-auth/', views.obtain_auth_token),  # drf自带的认证模式
    url(r'^login/', obtain_jwt_token),  # jwt的认证接口
]
~~~


### 视图中的配置,在需要登录验证的函数中加入
~~~Python
from rest_framework.permissions import IsAuthenticated
from rest_framework_jwt.authentication import JSONWebTokenAuthentication
from rest_framework.authentication import SessionAuthentication
from apps.utils.permissions import IsOwnerOrReadOnly

class UserFavViewSet(mixins.CreateModelMixin,mixins.ListModelMixin,mixins.RetrieveModelMixin,mixins.DestroyModelMixin,viewsets.GenericViewSet):
    permission_classes = (IsAuthenticated, IsOwnerOrReadOnly)  # 判断用户是否登录
    authentication_classes = (JSONWebTokenAuthentication, SessionAuthentication)  # jwt验证，session验证

    def get_queryset(self):
        return Model.objects.filter(user=self.request.user)  # 获取当前登录用户的数据
~~~
>JSONWebTokenAuthentication为jwt验证
>SessionAuthentication为session验证
>IsAuthenticated判断是否已登录
>IsOwnerOrReadOnly判断是否为对象拥有者，继承rest_framework.permissions.permissions

#### IsOwnerOrReadOnly代码 permissions.py
~~~Python
from rest_framework import permissions


class IsOwnerOrReadOnly(permissions.BasePermission):
    """
    对象级权限仅允许对象的所有者对其进行编辑
    假设模型实例具有`owner`属性。
    """

    def has_object_permission(self, request, view, obj):
        # 任何请求都允许读取权限，
        # 所以我们总是允许GET，HEAD或OPTIONS 请求.
        if request.method in permissions.SAFE_METHODS:
            return True

        # 示例必须要有一个名为`owner`的属性
        return obj.user == request.user
~~~
