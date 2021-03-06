# SDK Gradle集成方式

### 1、本地aar和jar包集成方式

1. 首先把aar和jar包放在libs目录下面

    2. 通过以下方式导入aar或者jar

```text
implementation fileTree(dir: 'libs', include: ['*.aar'])
//implementation fileTree(dir: 'libs', include: ['*.jar'])
```

### 2、远程aar集成方式

```text
implementation('cn.com.analysys:analysys-arkanalysys:latest.release')
```

### 3、其它须知

{% hint style="info" %}
4.4.0版本以下只支持jar集成方式，其它版本建议aar集成方式
{% endhint %}

{% hint style="info" %}
analysys-arkanalysys包含方舟sdk中的主要模块：analysys-core\(核心库不允许排除\)、analysys-encrypt（加密模块）、analysys-push（推送）、analysys-visual（可视化）、analysys-allgro（全埋点）；

如果不需要集成方舟sdk中的所有功能，可以单独排除某些模块，如需排除全埋点模块：

`compile('cn.com.analysys:analysys-arkanalysys:x.x.x') {   
        exclude group:'cn.com.analysys',module:'analysys-allgro'   
}`
{% endhint %}

{% hint style="info" %}
除主要模块外，还有其它功能模块，如mPaaS、React Native支持模块等，如果有相应需求需要单独集成，如需要支持React Native：

implementation\('cn.com.analysys:analysys-react-native:latest.release'\)
{% endhint %}

#### 

