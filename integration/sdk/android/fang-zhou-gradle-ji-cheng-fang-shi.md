# 方舟gradle集成方式

## aar集成方式介绍

### 本地aar和jar包集成方式

1. 首先把aar和jar包放在libs目录下面

    2. 通过以下方式导入aar

         方式一： compile fileTree\(dir: 'libs', include: \['_.jar','_.aar'\]\)

         方式二： compile files\('libs/analysys\_xxxx.jar'\) compile files\('libs/analysys\_xxxx.aar'\)

### 远程aar和jar包集成方式

compile\('cn.com.analysys:analysys-arkanalysys:x.x.x'\)

### compile、implementation区别

gradle老版本使用compile方式，gradle新版本推荐使用implementation方式

## 方舟gradle集成方式

### 4.4.0版本以上

#### **远程aar集成方式**

**集成依赖添加**

compile\('cn.com.analysys:analysys-arkanalysys:x.x.x'\)

注意:analysys-arkanalysys包含方舟sdk中的所有模块analysys\_core\_xxx.aar、analysys\_visual\_xxx.aar、analysys\_push\_xxx.aar、analysys\_encrypt\_xxx.aar、analysys\_allgro-xxx.aar；

**模块排除exclude**

如果不需要集成方舟sdk中的所有功能，可以单独排除某些模块，通过以下方式实现：

compile\('cn.com.analysys:analysys-arkanalysys:4.4.2'\) { exclude group:'cn.com.analysys',module:'analysys-xxxx' }

主要模块包含：analysys-core\(核心库不允许排除\)、analysys-encrypt（加密模块）、analysys-push（推送）、analysys-visual（可视化）、analysys-allgro（全埋点）

示例，不包含全埋点模块：

compile\('cn.com.analysys:analysys-arkanalysys:4.4.2'\) { exclude group:'cn.com.analysys',module:'analysys-allgro' }

