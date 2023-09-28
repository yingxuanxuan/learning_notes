# DRF



## 概念

* 什么是
  * DRF，Django REST framework
  * 自动生成可浏览的API
  * 支持身份验证策略OAuth 1a，OAuth2
  * 支持ORM和非ORM数据源的序列化



* 可选包
  * PyYAML、uritemplate，模式生成支持
  * Markdown，对可浏览API的Markdown支持
  * Pygments，对Markdown中的代码支持语法提示
  * django-filter，过滤支持
  * django-guardian，对象级权限支持



## 入门示例

* 安装

```sh
pip install djangorestframework
pip install markdown
pip install django-filter
```



* 将 `'rest_framework'` 添加到您的 `INSTALLED_APPS` 中

```py
# project/settings.py

INSTALLED_APPS = [
    ...
    'rest_framework',
]
```



* 为`browsable API`添加登录、注销视图

```py
# project/urls.py

urlpatterns = [
    ...
    path('api-auth/', include('rest_framework.urls'))
]
```



* 在django配置中添加DRF配置

```py
# project/settings.py

# 使用Django的标准权限`django.contrib.auth`
# 或对无权限的用户只读

REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.DjangoModelPermissionsOrAnonReadOnly'
    ]
}
```



* 将所需DRF代码先放在urls.py中执行

```py
from django.urls import path, include
from django.contrib.auth.models import User
from rest_framework import routers, serializers, viewsets


# 序列化器
# 定义User对象的API的表现形式
class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ['url', 'username', 'email', 'is_staff']

# 视图集合
# 定义视图行为
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer


# 注册自动路由
router = routers.DefaultRouter()
router.register(r'users', UserViewSet)


# 使用自动路由
urlpatterns = [
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```

