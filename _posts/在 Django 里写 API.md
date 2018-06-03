# Django + Vue + API 卡片式网页

> 在实战项目中学习，遇到不懂的再查看相关教程文档

- Django作为服务端架构
- 在 Django 中写 API 进行数据加载
  - 生成一个 localhost:9000/api/video/ 页面，参考 github.com/FatliTalk/vue_json_api/tree/gh-pages
  - vue 从以上 api 页面加载 api 数据（前端页面中的 `{{ }}` 中的内容由 vue 进行加载而不再是 Django ）
- Vue.js 进行页面渲染





# 在 Django 里写 API

> api 类似插线板，各种电器可以拿来用。如 Facebook 三个端（Web、iOS、Android）用的是同一套api



Models -> Views --json--> Vue.js（代替Template）



Models --序列化--> {字典}

> 序列化 (Serialization)将对象的状态信息转换为可以存储或传输的形式的过程。
>
> 我们把变量从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling，在其他语言中也被称之为serialization，marshalling，flattening等等，都是一个意思。
>
> 序列化之后，就可以把序列化后的内容写入磁盘，或者通过网络传输到别的机器上。
>
> 反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling。



{字典} --> json



```python
from rest_framework.decorators import api_view

# Python装饰器
@api_view(['GET'])
```



写好的API：

127.0.0.1:9000/api/video



使用API：

```javascript
vm = new Vue({
    el:"#app",
    data:{
        showMenu:false,
        chozen:'all',
        vids:[]
    },
    methods:{
        getData:function () {
            var self = this;
            reqwest({
                url:'http://127.0.0.1:9000/api/video/',
                // 这里请换成自己的端口，一般是8000
                type:'json',
                success:function (resp) {
                    self.vids = resp
                },

            })
        }
    },

    ...

})
```





# RESTful API 的增删改查

如何使用 RESTful API ？ http://www.django-rest-framework.org/tutorial/1-serialization/

遵循 REST 规范：

1. 网址对应资源：x.com/videos
2. 方法对应动作：
   - GET：查：x.com/videos
   - POST：增：x.com/videos
   - PUT：改：x.com/videos/1
   - DELETE：删：x.com/videos/1
3. 返回响应码：
   - 200：get成功
   - 201：post、put、delete被允许
   - 400：post、put、delete失败
   - 404：get失败，页面丢失





# RESTful API 的权限控制

### Session 和 token 的区别

Session 认证机制：

- 服务端认证（例如会议主办方签到表）

token（令牌）：

- RESTful API是无状态风格
- 客户端认证（例如与会者挂牌），放在Cookies中
  - 用到 [cookie.js](https://github.com/js-cookie/js-cookie) 库：3个主要方法：get、set、remove
- 由用户名与密码组合，通过特殊算法，生成一串16进制数字



##### 使用场景（用户有权使用我的资源么）：

类似知乎，没登录前只能查看5条，登录（验证token）后显示全部信息流。



##### 使用场景（用户有权删对方的文章么）：

权限允许情况：发布者owner与要执行删除操作的用户user为同一人

 





