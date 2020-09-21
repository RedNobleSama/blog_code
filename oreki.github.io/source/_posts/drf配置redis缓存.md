---
title: drf配置redis缓存
toc: true
mathjx: true
cover: /2019/04/15/drf配置redis缓存/head.png
tags:
  - Django
categories:
  - Python
  - Django
  - restframework
abbrlink: 25182
date: 2019-04-15 23:12:29
update:
---

### 安装django-redis,drf-extensions
>pip install django-redis
>pip install drf-extensions

### setting.py配置
~~~Python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
~~~

### 在视图函数中设置
~~~Python
from rest_framework_extensions.cache.mixins import CacheResponseMixin

class GoodlistViewSet(CacheResponseMixin, mixins.ListModelMixin, mixins.RetrieveModelMixin, viewsets.GenericViewSet): # 继承CacheResponseMixin
    # mixins.RetrieveModelMixin 详情
    """
    list:
        商品列表数据

    """
    queryset = Goods.objects.all()
    serializer_class = GoodsSerializer
    throttle_classes = (UserRateThrottle, AnonRateThrottle, )
    pagination_class = LargeResultsSetPagination
    filter_backends = (DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter)
    filter_class = GoodsFilter
    search_fields = ('=goods_sn', 'name', '=goods_num')
    ordering_fields = ('sold_num', 'add_time', 'shop_price')
~~~
