---
description: 以下接口生效依赖于推送模块，需集成推送SDK相关analysys_push_xxx.jar文件，请正确集成。
---

# 消息推送模块

## 简介

推送消息是一种常用的运营方法。在合适的时间把合适的内容通过合适的方式推送给合适的人群，不仅能促进用户活跃，也能极大提升用户对产品的体验。 易观推送现在支持极光、个推、百度、小米等平台。支持通过易观开发者平台设置以下三种类型的推送消息内容: 注意：该接口依赖于推送模块

* 下发通知，点击通知，打开宿主 App
* 下发通知，点击通知，打开宿主 App 的特定页面
* 下发通知，点击通知，触发打开特定的 URL 页面

首先，开发者需要在 App 中集成第三方推送系统的 SDK，并在 App 初始化过程中获取设备推送 ID，并保存在方舟分析的用户信息中。以下简要说明集成第三方推送系统的集成方式\(在 4.4 中可参考各平台相关说明及示例代码\)

## 设置设备推送 ID

该接口用于设置推送平台（provider）提供的设备推送 ID。 接口如下：

```java
setPushID(Context mContext, String provider,String pushId)
```

* mContext：上下文
* provider：推送服务方，易观SDK支持的平台,可从 PushProvider 中查询
* pushId：对应服务方的关于该移端端的唯一 ID

调用方法，以极光为例：

```java
AnalysysAgent.setPushID(context, PushProvider.JPUSH, 极光推送对应的设备ID);
```

对应表：

| 推送ID | 推送平台 | 调用方式 |
| :--- | :--- | :--- |
| GETUI | 个推 | PushProvider.GETUI |
| JPUSH | 极光 | PushProvider.JPUSH |
| BAIDU | 百度 | PushProvider.BAIDU |
| XIAOMI | 小米 | PushProvider.XIAOMI |

## 统计活动推广信息

### 追踪活动推广信息 API

追踪记录 provider 对应平台的消息推送的信息。 接口如下：

```java
trackCampaign (Context mContext, String campaign, boolean isClick)
```

* mContext：上下文
* campaign：该对象为一个 jsonObject 的 String 格式，其具体内容，为推送服务下发下来的 JsonString，直接传入 trackCampaign 接口即可
* isClick：活动通知是否点击打开

调用方法：

```java
AnalysysAgent.trackCampaign (context, campaignString, isClick);
```

#### 追加 PushListener 参数的追踪活动推广信息 API

```java
trackCampaign (Context mContext, String campaign, boolean isClick, PushListener listener)
```

* mContext：上下文
* campaign：该对象为一个 jsonObject 的 String 格式，其具体内容，为推送服务下发下来的 JsonString，直接传入 trackCampaign 接口即可
* isClick：活动通知是否点击打开
* listener：活动推送的后续自定义属性和行为的监听

调用方法：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
};
AnalysysAgent.trackCampaign(context, campaign, isClick, listener);
```

## 三方推送平台 SDK 集成及示例代码

首先，开发者需要在 App 中集成第三方推送系统的 SDK，并在 App 初始化过程中获取设备推送 ID，并保存在方舟分析的用户信息中。目前易观SDK支持极光、百度、小米、个推四家第三方推送统计的支持。以下简要说明集成第三方推送系统的集成方式。

### 极光推送

请参考极光推送[《Android SDK 文档》](https://docs.jiguang.cn/jpush/client/Android/android_sdk/)，将极光推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

极光推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受极光推送的广播，通过 cn.jpush.android.intent.REGISTRATION 消息获取 Registration Id。

```java
<receiver
    android:name="Your Receiver"
    android:enabled="true">
    <intent-filter>
        <action android:name="cn.jpush.android.intent.REGISTRATION" />
        <action android:name="cn.jpush.android.intent.MESSAGE_RECEIVED" />
        <action android:name="cn.jpush.android.intent.NOTIFICATION_RECEIVED" />
        <action android:name="cn.jpush.android.intent.NOTIFICATION_OPENED" />
        <category android:name="You package Name" />
    </intent-filter>
</receiver>
```

Receiver 在 onReceiver 回调中接收的 Intent 中JPushInterface.EXTRA\_REGISTRATION\_ID 的 Action 中获取为 Registration Id ，开发者在极光推送初始化完成后，在广播中获取JPushInterface.EXTRA\_REGISTRATION\_ID 的值为 Registration Id 并将他保存在方舟分析的用户信息中；JPushInterface.ACTION\_NOTIFICATION\_RECEIVED 的 Action 表示通知被接收；JPushInterface.ACTION\_NOTIFICATION\_OPENED 的Action 表示通知接收成功后，被用户点击。

```java
public void onReceive(Context context, Intent intent) {
    mContext = context;
    Bundle bundle = intent.getExtras();
    if (JPushInterface.ACTION_REGISTRATION_ID.equals(intent.getAction())) {
        String regId = bundle.getString(JPushInterface.EXTRA_REGISTRATION_ID);
        Log.d(TAG, "接收Registration Id: " + regId);
        //易观打开推送接口
        AnalysysAgent.setPushID(context, PushProvider.JPUSH, regId);
    } else if (JPushInterface.ACTION_NOTIFICATION_RECEIVED.equals(intent.getAction())) {
        int notifactionId = bundle.getInt(JPushInterface.EXTRA_NOTIFICATION_ID);
        Log.d(TAG, "接收到推送下来的通知的ID: " + notifactionId);
        //接收到Push的通知
        AnalysysAgent.trackCampaign(context,bundle.getString(JPushInterface.EXTRA_EXTRA),false);
    } else if (JPushInterface.ACTION_NOTIFICATION_OPENED.equals(intent.getAction())) {
        Log.d(TAG, "用户点击打开了通知");
        //易观添加活动推广接口，点击了Push推下来的通知
        AnalysysAgent.trackCampaign(context,bundle.getString(JPushInterface.EXTRA_EXTRA),true);
    }
}
```

当统计消息接收成功或统计点击通知后需要执行自定义行为是，可实现 PushListener 接口，将实现对象传入 trackCampaign，例如要在点击通知后，输出日志，如下调用：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
    };
AnalysysAgent.trackCampaign(context,bundle.getString(JPushInterface.EXTRA_EXTRA),true,listener);
```

### 个推推送

请参考个推推送[《Android SDK 文档》](http://docs.getui.com/getui/start/andorid/)，将个推推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

个推推送使用唯一的 clientid 标示设备。开发者需要在 Manifest 中定义 继承GTIntentService 的自定义 Service 接受个推推送的消息。

```java
<service android:name="Your service" />
```

在继承自 GTIntentService 的 Service 中复写回调方法,在 onReceiveClientId回调中调用 setPushID，将他保存在方舟分析的用户信息中;在 onReceiveMessageData 回调接口中，调用 trackCampaign 接口设置用户点击通知后事件追踪。

```java
@Override
public void onReceiveClientId(Context context, String clientid) {
    Log.e(TAG, "onReceiveClientId -> " + "clientid = " + clientid);
    AnalysysAgent.setPushID(context, PushProvider.GETUI,clientid);
}

@Override
public void onReceiveMessageData(Context context, GTTransmitMessage gtTransmitMessage) {
    Log.e(TAG,"onReceiveMessageData -> " + "msg:" + gtTransmitMessage.toString());
    byte[] payload = gtTransmitMessage.getPayload();
    String data = new String(payload);
    AnalysysAgent.trackCampaign(context,data,true);
}
```

当统计点击通知后需要执行自定义行为时，可实现 PushListener 接口，将实现对象传入 trackCampaign，例如要在点击通知后，输出日志，如下调用：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
    };
AnalysysAgent.trackCampaign(context,data,true,listener);
```

### 百度云推送

请参考百度云推送[《Android SDK 文档》](http://push.baidu.com/doc/android/api)，将百度云推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

百度云推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受百度云推送的广播，通过 com.baidu.android.pushservice.action.RECEIVE 消息获取 Registration Id。

```java
<receiver android:name="Your Receiver">
    <intent-filter>
        <!-- 接收push消息 -->
        <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
        <!-- 接收bind,unbind,fetch,delete等反馈消息 -->
        <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
        <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
    </intent-filter>
</receiver>
```

在百度的 Receiver 在自定义了获取百度推送 ID 的回调接口 onBind，获取Channel Id，在该回调中调用 setPushID，将他保存在方舟分析的用户信息中;在 onNotificationArrived 回调接口中，调用 trackCampaign 接口设置接收通知成功;在 onNotificationClicked 回调接口中，调用 trackCampaign 接口设置用户点击通知后事件追踪。

```java
@Override
public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
    Log.d(TAG, responseString);
    if (errorCode == 0) {
        // 绑定成功
        Log.d(TAG, "绑定成功");
        AnalysysAgent.setPushID(context, PushProvider.BAIDU,channelId);
    }
}
@Override
public void onNotificationArrived(Context context, String title,
                                    String description, String customContentString) {
    Log.d(TAG, notifyString);
    // 你可以參考 onNotificationClicked中的提示从自定义内容获取具体值
    AnalysysAgent.trackCampaign(context,customContentString,false);
}

@Override
public void onNotificationClicked(Context context, String title,
                                    String description, String customContentString) {
    Log.d(TAG, notifyString);
    AnalysysAgent.trackCampaign(context,customContentString,true);
}
```

当统计点击通知后需要执行自定义行为时，可实现 PushListener 接口，将实现对象传入trackCampaign，例如要在点击通知后，输出日志，如下调用：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
    };
AnalysysAgent.trackCampaign(context,customContentString,true,listener);
```

### 小米推送

请参考小米推送[《Android SDK 文档》](https://dev.mi.com/console/doc/detail?pId=41)，将小米推送送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

小米推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受小米推送送的广播，通过广播消息获取 Registration Id。

```java
<receiver
    android:name="Your Receiver"
    android:exported="true">
    <intent-filter>
        <action android:name="com.xiaomi.mipush.RECEIVE_MESSAGE" />
    </intent-filter>
    <intent-filter>
        <action android:name="com.xiaomi.mipush.MESSAGE_ARRIVED" />
    </intent-filter>
    <intent-filter>
        <action android:name="com.xiaomi.mipush.ERROR" />
    </intent-filter>
</receiver>
```

广播消息的 MiPushCommandMessage 参数中可以获取到 Registration Id ，开发者在小米推送初始化完成后，在广播中获取 Registration Id 并将他保存在方舟分析的用户信息中; 在回调方法 onNotificationMessageArrived 中记录记录 App 推送成功事件；在 onNotificationMessageClicked 回调中记录用户点击消息打开 App 或执行其他行为的事件

```java
@Override
public void onReceiveRegisterResult(Context context, MiPushCommandMessage message) {
    String command = message.getCommand();
    List<String> arguments = message.getCommandArguments();
    String cmdArg = ((arguments != null && arguments.size() > 0) ? arguments.get(0) : null);
    if (MiPushClient.COMMAND_REGISTER.equals(command)) {
        if (message.getResultCode() == ErrorCode.SUCCESS) {
            AnalysysAgent.setPushID(context, PushProvider.XIAOMI,cmdArg);
        }
    }
}
@Override
public void onNotificationMessageArrived(Context context, MiPushMessage message) {
    mMessage = message.getContent();
    AnalysysAgent.trackCampaign(context,mMessage,false);
}
@Override
public void onNotificationMessageClicked(Context context, MiPushMessage message) {
    mMessage = message.getContent();
    AnalysysAgent.trackCampaign(context,mMessage,true);
}
```

当统计点击通知后需要执行自定义行为时，可实现 PushListener接口，将实现对象传入 trackCampaign，例如要在点击通知后，输出日志，如下调用：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
    };
AnalysysAgent.trackCampaign(context,mMessage,true,listener);
```

