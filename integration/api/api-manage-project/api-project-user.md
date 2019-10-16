---
description: 4.4.2版本中新增
---

# 权限管理

## 1. 获取项目角色元数据列表

### 1.1 接口地址

> 【GET】 /uba/manage/project/roles

### 1.2 请求参数示例

无

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 1.3 返回结果示例

```java
[
    {
        //角色ID
        "id":6,
        //角色名称
        "name":"普通成员"
    }
]
```

### 1.4 接口调用示例

```java
curl -H "token:1b554f363d56238bf33a201620f2e9a9" -H "appKey:31abd9593e9983ec" 'http://127.0.0.1:4005/uba/manage/project/roles'
```

## 2. 添加项目成员

传入的邮箱必须是在方舟产品中注册成功的用户，如果返回未注册，需要先调用注册接口。

如果项目已经添加过用户，用户对应的角色会被替换中接口中指定的角色。

### 2.1 接口地址

> 【POST】 /uba/manage/project/users

### 2.2 请求参数示例

```java
{
    "users":[
        {
            "email": "api@analysys.com.cn",
            "role": 6
        },
        {
            "email": "api2@analysys.com.cn",
            "role": 6
        }
    ]
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
>
> **操作用户**：接口如果需要记录操作人，URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

#### 2.2.1 入参说明

| 参数名称 | 类型 | 必填 | 说明 | 枚举 |
| :--- | :--- | :--- | :--- | :--- |
| users | List | Y | 要添加的用户对象，备注:Body参数 |  |
| email | String | Y | 要授权项目的用户邮箱 |  |
| role | Long | Y | 用户在项目中的角色ID |  |

### 2.3 返回结果示例

```java
{
    //返回success说明操作成功
    "success":0
}
```

### 2.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:1b554f363d56238bf33a201620f2e9a9" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "users":[
        {
            "email": "api@analysys.com.cn",
            "role": 6
        },
        {
            "email": "api2@analysys.com.cn",
            "role": 6
        }
    ]
}' http://127.0.0.1:4005/uba/manage/project/users?loginUser=admin@analysys.com.cn2
```

## 3. 删除项目成员

### 3.1 接口地址

> 【DELETE】 /uba/manage/project/users

### 3.2 请求参数示例

```java
{
    "emails":[
        "api@analysys.com.cn",
        "api2@analysys.com.cn"
    ]
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
>
> **操作用户**：接口如果要记录操作人，URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

#### 3.2.1 入参说明

| 参数名称 | 类型 | 必填 | 说明 | 枚举 |
| :--- | :--- | :--- | :--- | :--- |
| email | List&lt;String&gt; | Y | 要移除的用户邮箱集合 |  |

### 3.3 返回结果示例

```java
{
    //返回success说明操作成功
    "success":0
}
```

### 3.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:1b554f363d56238bf33a201620f2e9a9" -H "appKey:31abd9593e9983ec" -X DELETE --data '{
    "emails":[
        "api@analysys.com.cn",
        "api2@analysys.com.cn"
    ]
}' http://127.0.0.1:4005/uba/manage/project/users?loginUser=admin@analysys.com.cn
```

