# 多渠道打包

## 1. 多渠道打包

1、 修改AndroidManifest.xml配置信息 示例：

```text
<meta-data
        android:name="ANALYSYS_APPKEY"
        android:value="demobank631"/>
<meta-data
    android:name="ANALYSYS_CHANNEL"
    android:value="${ANALYSYS_CHANNEL_VALUE}"/>
```

渠道value的值修改为`${ANALYSYS_CHANNEL_VALUE}`

2、 在build.gradle文件内添加渠道信息 例如需要打包的渠道为小米、百度、豌豆荚，则配置如下：

```text
方式 1
android {
    defaultConfig {
        flavorDimensions "versionCode"
    }
    productFlavors {
       xiaomi {
          manifestPlaceholders = [ANALYSYS_CHANNEL_VALUE: "xiaomi"]
        }
        baidu {
          manifestPlaceholders = [ANALYSYS_CHANNEL_VALUE: "baidu"]
        }
        wandoujia {
          manifestPlaceholders = [ANALYSYS_CHANNEL_VALUE: "wandoujia"]
        }

      }
}
// 方式 2
android {
    defaultConfig {
        flavorDimensions "versionCode"
    }
    productFlavors {
        kuan {}
        xiaomi {}
        qh360 {}
        baidu {}
        wandoujia{}
        productFlavors.all { flavor ->
            flavor.manifestPlaceholders = [ANALYSYS_CHANNEL_VALUE: name]
        }
    }
}
```

以上两种配置方式均可。

3、开发工具打apk包 在AndroidStudio菜单栏点击Build菜单–&gt;Generate signed APK–&gt;选择key并输入密码，选择apk包的存储位置并选择需要的渠道。

注意：如果打出的apk包名称中携带unsigned信息，如：app-baidu-release-unsigned.apk，可以试试打包的时候手动输入一下签名信息的密码。

4、 命令行打apk包 还需在build.gradle文件中配置如下签名信息：

```text
android {
    signingConfigs {
        release {
          keyAlias "analysys"
          keyPassword "analysys" //签名密码
          storeFile file("../keyStore") //签名文件路径
          storePassword "analysys"

        }
      }
}
```

执行命令`./gradlew assembleRelease`,此时在`app/build/output/apk`目录下会生成相应渠道的apk包。 注意：如果打出的apk包名称中携带unsigned信息，如：app-baidu-release-unsigned.apk， 则表示apk包无签名信息，则需要在build.gradle文件中添加如下内容：

```text
buildTypes {
    release {
      signingConfig signingConfigs.release
    }
  }
```

如果想打指定渠道的包可以执行如下格式命令： `./gradlew assemble + build.gradle中配置渠道名称 + release`

如需要打豌豆荚渠道apk包，则执行如下命令：

```text
./gradlew assembleWandoujiaRelease
```

