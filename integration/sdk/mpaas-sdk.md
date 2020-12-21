---
description: mPaaS架构应用如何使用方舟sdk采集数据
---

# mPaaS SDK

## Android端

将sdk中的aar文件拷贝到portal和bundle工程的libs文件夹中，portal为implementation，bundle为compileOnly

```text
portal：implementation fileTree(dir: 'libs', include: ['*.aar'])
bundle：compileOnly fileTree(dir: 'libs', include: ['*.aar'])
```

{% hint style="info" %}
sdk版本需要4.5.4以上
{% endhint %}

在portal工程中添加provider

```text
<provider
    android:name="com.analysys.database.AnsContentProvider"
    android:authorities="[应用包名].AnsContentProvider"
    android:enabled="true"
    android:exported="false"
    tools:replace="android:authorities"/>
```

将全埋点插件添加到portal和bundle工程中

```text
项目根build.gradle:
dependencies {
        ...
        classpath 'cn.com.analysys:analysys-allgro-plugin:1.1.2'
 }
 
项目模块build.gradle:
apply plugin: 'com.analysys.android.plugin'
```

在 LauncherActivityAgent 的 preInit 函数中初始化方舟sdk

```text
@Override
public void preInit() {
    super.preInit();

    // 初始化方舟sdk
    initAnalysys();

    // 如果使用了H5容器，需要调用此接口初始化Hybrid相关功能
    AnalysysMpaas.init();
}

public static final int DEBUG_MODE = 2;
public static final String APP_KEY = "heatmaptest0916";
public static final String UPLOAD_URL = "http://192.168.220.105:8089";
private static final String SOCKET_URL = "ws://192.168.220.105:9091";
private static final String CONFIG_URL = "http://192.168.220.105:8089";

private void initAnalysys() {
    Context ctx = getApplicationContext();
    AnalysysAgent.setDebugMode(ctx, DEBUG_MODE);
    //  设置 debug 模式，值：0、1、2
    AnalysysConfig config = new AnalysysConfig();
    // 设置key(目前使用电商demo的key)
    config.setAppKey(APP_KEY);
    // 设置渠道
    config.setChannel("AnalysysDemo");
    // 设置追踪新用户的首次属性
    config.setAutoProfile(true);
    // 设置使用AES加密
    config.setEncryptType(EncryptEnum.AES);
    // 设置服务器时间校验
    config.setAllowTimeCheck(true);
    // 时间最大允许偏差为5分钟
    config.setMaxDiffTimeInterval(5 * 60);
    // 开启渠道归因
    config.setAutoInstallation(true);
    // 热图数据采集（默认关闭）
    config.setAutoHeatMap(true);
    // pageView自动上报总开关（默认开启）
    config.setAutoTrackPageView(true);
    // fragment-pageView自动上报开关（默认关闭）
    config.setAutoTrackFragmentPageView(true);
    // 点击自动上报开关（默认关闭）
    config.setAutoTrackClick(true);

    config.setAutoTrackCrash(true);

    config.setAutoTrackDeviceId(true);
    // 初始化
    AnalysysAgent.init(ctx, config);
    AnalysysAgent.setUploadNetworkType(AnalysysAgent.AnalysysNetworkType.AnalysysNetworkWIFI);
    // 设置数据上传/更新地址
    AnalysysAgent.setUploadURL(ctx, UPLOAD_URL);
    // 设置 WebSocket 连接 Url
    AnalysysAgent.setVisitorDebugURL(ctx, SOCKET_URL);
    // 设置配置下发Url
    AnalysysAgent.setVisitorConfigURL(ctx, CONFIG_URL);
}
```

