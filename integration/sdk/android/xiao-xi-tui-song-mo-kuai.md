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

在继承自 GTIntentService 的 Service 中复写回调方法,在 onReceiveClientId回调中调用 setPushID，将他保存在方舟分析的用户信息中；在onNotificationMessageArrived回调接口中调用 trackCampaign 接口设置通知展现追踪；在onNotificationMessageClicked回调接口中调用 trackCampaign 接口设置通知点击追踪；在 onReceiveMessageData 回调接口中调用 trackCampaign 接口设置透传消息追踪。

```java
@Override
public void onReceiveClientId(Context context, String clientid) {
    Log.e(TAG, "onReceiveClientId -> " + "clientid = " + clientid);
    AnalysysAgent.setPushID(context, PushProvider.GETUI,clientid);
}

@Override
public void onNotificationMessageArrived(Context context, GTNotificationMessage
        gtNotificationMessage) {
    Log.e(TAG, "onNotificationMessageArrived -> " + "msg:" + gtNotificationMessage.toString());
    String data = gtNotificationMessage.getContent();
    AnalysysAgent.trackCampaign(context, data, false, listener);
}

@Override
public void onNotificationMessageClicked(Context context,
                                         GTNotificationMessage gtNotificationMessage) {
    Log.e(TAG, "onNotificationMessageClicked -> " + "msg:" + gtNotificationMessage.toString());
    String data = gtNotificationMessage.getContent();
    AnalysysAgent.trackCampaign(context, data, true, listener);
}

@Override
public void onReceiveMessageData(Context context, GTTransmitMessage gtTransmitMessage) {
    Log.e(TAG,"onReceiveMessageData -> " + "msg:" + gtTransmitMessage.toString());
    byte[] payload = gtTransmitMessage.getPayload();
    String data = new String(payload);
    AnalysysAgent.trackCampaign(context,data,false);
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

### 阿里云推送

请参考阿里云推送[《Android SDK 文档》](https://help.aliyun.com/document_detail/51056.html?spm=a2c4g.11186623.6.587.62ed527eQyARCV)，将阿里云推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

阿里云推送使用唯一的 DeviceId 标示设备。开发者需要在初始化的时候设置setPushID，将DeviceId保存在方舟分析的用户信息中

```java
PushServiceFactory.init(mContext);
        CloudPushService pushService = PushServiceFactory.getCloudPushService();
        if (pushService.getDeviceId() != null) {
            AnalysysAgent.setPushID(mContext, PushProvider.ALIYUN, pushService.getDeviceId());
        }
        pushService.register(mContext, new CommonCallback() {
            @Override
            public void onSuccess(String response) {
            }
            @Override
            public void onFailed(String errorCode, String errorMessage) {
            }
        });

```

创建消息接收Receiver，继承自com.alibaba.sdk.android.push.MessageReceiver，并在对应回调中添加业务处理逻辑，在onNotification中调用trackCampaign设置通知到达成功；在onNotificationOpened中调用trackCapmaign设置通知点击成功，可参考以下代码：

```java
public class MyMessageReceiver extends MessageReceiver {
    // 消息接收部分的LOG_TAG
    public static final String REC_TAG = "receiver";
    @Override
    public void onNotification(Context context, String title, String summary, Map<String, String> extraMap) {
        // TODO 处理推送通知
        Log.e("MyMessageReceiver", "Receive notification, title: " + title + ", summary: " + summary + ", extraMap: " + extraMap);
				try {
            JSONObject jsonObject = new JSONObject();
            if (extraMap != null) {
                jsonObject.put(Constants.PUSH_KEY_INFO, extraMap.get(Constants.PUSH_KEY_INFO));
            }
            AnalysysAgent.trackCampaign(context,jsonObject.toString(),false);
        } catch (JSONException e) {
            e.printStackTrace();
        }
}
    @Override
    public void onMessage(Context context, CPushMessage cPushMessage) {
            Log.e("MyMessageReceiver", "onMessage, messageId: " + cPushMessage.getMessageId() + ", title: " + cPushMessage.getTitle() + ", content:" + cPushMessage.getContent());
    }
    @Override
    public void onNotificationOpened(Context context, String title, String summary, String extraMap) {
        Log.e("MyMessageReceiver", "onNotificationOpened, title: " + title + ", summary: " + summary + ", extraMap:" + extraMap);
    		try {
            JSONObject extraJson = new JSONObject(extraMap);
            JSONObject jsonObject = new JSONObject();
            if (extraMap != null) {
                jsonObject.put(Constants.PUSH_KEY_INFO, extraJson.get(Constants.PUSH_KEY_INFO));
            }
            AnalysysAgent.trackCampaign(context,jsonObject.toString(),true);
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }
    @Override
    protected void onNotificationClickedWithNoAction(Context context, String title, String summary, String extraMap) {
        Log.e("MyMessageReceiver", "onNotificationClickedWithNoAction, title: " + title + ", summary: " + summary + ", extraMap:" + extraMap);
    }
    @Override
    protected void onNotificationReceivedInApp(Context context, String title, String summary, Map<String, String> extraMap, int openType, String openActivity, String openUrl) {
        Log.e("MyMessageReceiver", "onNotificationReceivedInApp, title: " + title + ", summary: " + summary + ", extraMap:" + extraMap + ", openType:" + openType + ", openActivity:" + openActivity + ", openUrl:" + openUrl);
    }
    @Override
    protected void onNotificationRemoved(Context context, String messageId) {
        Log.e("MyMessageReceiver", "onNotificationRemoved");
    }
}

```

### 信鸽推送

请参考信鸽推送[《Android SDK 文档》](https://xg.qq.com/docs/android_access/upgrade_guide.html)，将信鸽推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

创建消息接收Receiver，继承自com.tencent.android.tpush.XGPushBaseReceiver，并在对应回调中添加业务处理逻辑。信鸽推送使用唯一的 token 标示设备。开发者需要在 Manifest 中定义 Receiver 接受信鸽推送的广播，通过 XGPushBaseReceiver广播类获取 token，在onRegisterResult回调中通过setPushID来把token信息保存在方舟分析的用户信息中。

在onNotifactionShowedResult中调用trackCampaign设置通知展示成功；在onNotifactionClickedResult中调用trackCapmaign设置通知点击成功，可参考以下代码：

public class XgMessageReceiver extends XGPushBaseReceiver { private Intent intent = new Intent\("com.qq.xgdemo.activity.UPDATE\_LISTVIEW"\); public static final String LogTag = "TPushReceiver";

```text
private void show(Context context, String text) {
    Toast.makeText(context, text, Toast.LENGTH_SHORT).show();
}

// 通知展示
@Override
public void onNotifactionShowedResult(Context context,
                                      XGPushShowedResult notifiShowedRlt) {
    if (context == null || notifiShowedRlt == null) {
        return;
    }
    XGNotification notific = new XGNotification();
    notific.setMsg_id(notifiShowedRlt.getMsgId());
    notific.setTitle(notifiShowedRlt.getTitle());
    notific.setContent(notifiShowedRlt.getContent());
    // notificationActionType==1为Activity，2为url，3为intent
    notific.setNotificationActionType(notifiShowedRlt
            .getNotificationActionType());
    //Activity,url,intent都可以通过getActivity()获得
    notific.setActivity(notifiShowedRlt.getActivity());
    notific.setUpdate_time(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")
            .format(Calendar.getInstance().getTime()));
```

// NotificationService.getInstance\(context\).save\(notific\); context.sendBroadcast\(intent\); show\(context, "您有1条新消息, " + "通知被展示 ， " + notifiShowedRlt.toString\(\)\); Log.d\("LC", "+++++++++++++++++++++++++++++展示通知的回调"\);

```text
public class XgMessageReceiver extends XGPushBaseReceiver {
    private Intent intent = new Intent("com.qq.xgdemo.activity.UPDATE_LISTVIEW");
    public static final String LogTag = "TPushReceiver";

    private void show(Context context, String text) {
        Toast.makeText(context, text, Toast.LENGTH_SHORT).show();
    }

    // 通知展示
    @Override
    public void onNotifactionShowedResult(Context context,
                                          XGPushShowedResult notifiShowedRlt) {
        if (context == null || notifiShowedRlt == null) {
            return;
        }
        XGNotification notific = new XGNotification();
        notific.setMsg_id(notifiShowedRlt.getMsgId());
        notific.setTitle(notifiShowedRlt.getTitle());
        notific.setContent(notifiShowedRlt.getContent());
        // notificationActionType==1为Activity，2为url，3为intent
        notific.setNotificationActionType(notifiShowedRlt
                .getNotificationActionType());
        //Activity,url,intent都可以通过getActivity()获得
        notific.setActivity(notifiShowedRlt.getActivity());
        notific.setUpdate_time(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")
                .format(Calendar.getInstance().getTime()));
//        NotificationService.getInstance(context).save(notific);
        context.sendBroadcast(intent);
        show(context, "您有1条新消息, " + "通知被展示 ， " + notifiShowedRlt.toString());
        Log.d("LC", "+++++++++++++++++++++++++++++展示通知的回调");

        notifiShowedRlt.getCustomContent();

        Log.d(LogTag, "customContent:" + notifiShowedRlt.getCustomContent() + ",content:" + notifiShowedRlt.getContent());


        if (!TextUtils.isEmpty(notifiShowedRlt.getCustomContent())) {
            AnalysysAgent.trackCampaign(context,notifiShowedRlt.getCustomContent() , false);
        }
    }

    //反注册的回调
    @Override
    public void onUnregisterResult(Context context, int errorCode) {
        if (context == null) {
            return;
        }
        String text = "";
        if (errorCode == XGPushBaseReceiver.SUCCESS) {
            text = "反注册成功";
        } else {
            text = "反注册失败" + errorCode;
        }
        Log.d(LogTag, text);
        show(context, text);

    }

    //设置tag的回调
    @Override
    public void onSetTagResult(Context context, int errorCode, String tagName) {
        if (context == null) {
            return;
        }
        String text = "";
        if (errorCode == XGPushBaseReceiver.SUCCESS) {
            text = "\"" + tagName + "\"设置成功";
        } else {
            text = "\"" + tagName + "\"设置失败,错误码：" + errorCode;
        }
        Log.d(LogTag, text);
        show(context, text);

    }

    //删除tag的回调
    @Override
    public void onDeleteTagResult(Context context, int errorCode, String tagName) {
        if (context == null) {
            return;
        }
        String text = "";
        if (errorCode == XGPushBaseReceiver.SUCCESS) {
            text = "\"" + tagName + "\"删除成功";
        } else {
            text = "\"" + tagName + "\"删除失败,错误码：" + errorCode;
        }
        Log.d(LogTag, text);
        show(context, text);

    }

    // 通知点击回调 actionType=1为该消息被清除，actionType=0为该消息被点击。此处不能做点击消息跳转，详细方法请参照官网的Android常见问题文档
    @Override
    public void onNotifactionClickedResult(Context context,
                                           XGPushClickedResult message) {
        Log.e("LC", "+++++++++++++++ 通知被点击 跳转到指定页面。");
        NotificationManager notificationManager = (NotificationManager) context
                .getSystemService(Context.NOTIFICATION_SERVICE);
        notificationManager.cancelAll();
        if (context == null || message == null) {
            return;
        }
        String text = "";
        if (message.getActionType() == XGPushClickedResult.NOTIFACTION_CLICKED_TYPE) {
            // 通知在通知栏被点击啦。。。。。
            // APP自己处理点击的相关动作
            // 这个动作可以在activity的onResume也能监听，请看第3点相关内容
            text = "通知被打开 :" + message;
        } else if (message.getActionType() == XGPushClickedResult.NOTIFACTION_DELETED_TYPE) {
            // 通知被清除啦。。。。
            // APP自己处理通知被清除后的相关动作
            text = "通知被清除 :" + message;
        }
        Toast.makeText(context, "广播接收到通知被点击:" + message.toString(),
                Toast.LENGTH_SHORT).show();
        // 获取自定义key-value
        String customContent = message.getCustomContent();
        if (customContent != null && customContent.length() != 0) {
            try {
                JSONObject obj = new JSONObject(customContent);
                // key1为前台配置的key
                if (!obj.isNull("key")) {
                    String value = obj.getString("key");
                    Log.d(LogTag, "get custom value:" + value);
                }
                // ...
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
        // APP自主处理的过程。。。
        Log.d(LogTag, text);
        show(context, text);

        Log.d(LogTag, "customContent:" + message.getCustomContent() + ",content:" + message.getContent());

        if (!TextUtils.isEmpty(message.getCustomContent())) {
            AnalysysAgent.trackCampaign(context, message.getCustomContent(), true);
        }


    }

    //注册的回调
    @Override
    public void onRegisterResult(Context context, int errorCode,
                                 XGPushRegisterResult message) {
        // TODO Auto-generated method stub
        if (context == null || message == null) {
            return;
        }
        String text = "";
        if (errorCode == XGPushBaseReceiver.SUCCESS) {
            text = message + "注册成功";
            // 在这里拿token
            String token = message.getToken();
        } else {
            text = message + "注册失败错误码：" + errorCode;
        }
        Log.d(LogTag, text);
        show(context, text);

        if (!TextUtils.isEmpty(message.getToken())) {
            AnalysysAgent.setPushID(context, PushProvider.XINGE, message.getToken());
        }
    }

    // 消息透传的回调
    @Override
    public void onTextMessage(Context context, XGPushTextMessage message) {
        // TODO Auto-generated method stub
        String text = "收到消息:" + message.toString();
        // 获取自定义key-value
        String customContent = message.getCustomContent();
        if (customContent != null && customContent.length() != 0) {
            try {
                JSONObject obj = new JSONObject(customContent);
                // key1为前台配置的key
                if (!obj.isNull("key")) {
                    String value = obj.getString("key");
                    Log.d(LogTag, "get custom value:" + value);
                }
                // ...
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
        Log.d("LC", "++++++++++++++++透传消息");
        // APP自主处理消息的过程...
        Log.d(LogTag, text);
        show(context, text);
    }

}

```



