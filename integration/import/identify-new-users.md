# 数据导入FAQ

## 1、数据导入后原有新访用户应该如何标识？

方舟通过 `$first_visit_time` 来记录用户所使用的设备首次访问的时间。正常情况下，客户端SDK会通过一个标识来区分这个设备是否第一次访问，并且只会在发现设备第一次访问的情况的时机才会自动上报这个属性。

对于导入历史数据的情况，若历史用户数据中包含首次访问的时间，则可以通过设置用户的 `$first_visit_time` 字段来将用户的首次访问事件设置为历史上真实的时间。

以 Python SDK 为例：

```python
python_sdk_platform = "android"
alias_id = "zhangsan"
properties = {
           "$first_visit_time" : 1572676233813
        }
eguan.profile_set_once(alias_id, properties, python_sdk_platform, is_login=True)
```

{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

