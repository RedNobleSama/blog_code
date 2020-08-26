---
title: throttle设置api的访问速率
toc: true
mathjx: true
cover: /2019/04/15/throttle设置api的访问速率/head.png
date: 2019-04-15 23:12:05
update:
tags: ['Django']
categories:
  - Python
  - Django
  - restframework

---
### setting.py设置
在setting.py中添加
~~~Python
REST_FRAMEWORK = {
  'DEFAULT_THROTTLE_CLASSES': (
      'rest_framework.throttling.AnonRateThrottle',  
      'rest_framework.throttling.UserRateThrottle'   
  ),
  'DEFAULT_THROTTLE_RATES': {
        'anon': '20000/day',
        'user': '30000/day'
  }
}
~~~
DEFAULT_THROTTLE_CLASSES是判断类型
>AnonRateThrottle是未登录情况下，判断IP地址
>UserRateThrottle是登录情况下，判断token

DEFAULT_THROTTLE_RATES是限速策略。

### 在视图函数中设置
在需要限速的api中添加throttle_classes
~~~Python
from rest_framework.throttling import UserRateThrottle, AnonRateThrottle

class GoodlistViewSet(CacheResponseMixin, mixins.ListModelMixin, mixins.RetrieveModelMixin, viewsets.GenericViewSet):
    # mixins.RetrieveModelMixin 详情
    """
    list:
        商品列表数据

    """
    queryset = Goods.objects.all()
    serializer_class = GoodsSerializer
    throttle_classes = (UserRateThrottle, AnonRateThrottle, ) # api限速
    pagination_class = LargeResultsSetPagination
    filter_backends = (DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter)
    filter_class = GoodsFilter
    search_fields = ('=goods_sn', 'name', '=goods_num')
    ordering_fields = ('sold_num', 'add_time', 'shop_price')
  ~~~
