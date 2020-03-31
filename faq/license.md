# License 许可

## License 是什么？

License 是方舟系统的使用许可，只有输入License方可激活系统来使用。

{% hint style="info" %}
License 中包含的信息：

部署的服务器机器码（V4.6版本新增）、企业可使用的时长、可创建的项目个数、可处理的事件量、可部署的节点数
{% endhint %}

当部署环境变更、系统到期、项目数、事件量或节点数超过限额时，需要使用新的 License 来重新激活、续期、扩容。

## 如何使用License？

### 首次激活

部署完成后，系统会要求输入 License 

{% hint style="info" %}
**如何获取激活 License？**

Argo 用户：在方舟官网 [获取License页面](https://ark.analysys.cn/license.html) 自主申请 

商业版客户：部署完成后复制机器码发送给对接的项目经理，会有专人生成并发送给您采购期的License，也可以使用平台管理员帐号登录易观方舟官网，在 **个人中心-我的 License** 中获取
{% endhint %}

![&#x6FC0;&#x6D3B;&#x7A97;&#x53E3;](../.gitbook/assets/image%20%2867%29.png)

### 提前续期/扩容

对于使用时长和事件量系统会在临近限额前提前预警

* 当临近到期前30、15、7、3、2、1 天时
* 当事件量达到限额的 80%、90%、95%、99%时

系统会发出预警消息（登录后可见弹窗消息或在 消息中心 查看） 

![&#x9884;&#x8B66;&#x6D88;&#x606F;](../.gitbook/assets/image%20%2841%29.png)

平台管理员可以登录进入企业管理后台 **企业概览-当前计划** 中查看使用时长限制、项目数、事件量、节点数限额和当前用量，根据当前使用情况来确定是否提前续期/扩容

![&#x7EED;&#x671F;/&#x6269;&#x5BB9;](../.gitbook/assets/image%20%28211%29.png)

### 到期、节点超限后续期/扩容

若已经到期或擅自扩展部署节点，则系统无法继续使用，需要在弹窗中输入新的License来续期/扩容

![&#x6388;&#x6743;&#x5230;&#x671F;](../.gitbook/assets/image%20%28271%29.png)

![&#x8282;&#x70B9;&#x8D85;&#x9650;](../.gitbook/assets/image%20%28260%29.png)

### 事件用量超限后扩容

当事件用量超限后，系统将会自动关闭数据流，会弹窗提醒扩容

若在有效期内，节点也未超限的情况下，事件量超出限额仍然可以继续使用，只是会停止新的数据处理，超出一定范围新数据将丢失，无法找回，所以避免影响使用，输入新的License来扩容

![&#x4E8B;&#x4EF6;&#x91CF;&#x8D85;&#x9650;](../.gitbook/assets/image%20%28150%29.png)

### 部署机器变更后重新认证

当变更了部署机器后，可能会导致核心调度节点变更，需要使用新的机器码生成新的 License 来认证

![&#x53D8;&#x66F4;&#x673A;&#x5668;](../.gitbook/assets/image%20%28122%29.png)

{% hint style="info" %}
如何获取新的 License？

Argo 用户：登录易观官网，进入**个人中心 - 我的 License** 中自主申请

商业版客户：联系您的专属客户服务团队，获取新的 License ，仍然使用平台管理员帐号登录易观方舟官网，进入**个人中心 - 我的 License**  中查看
{% endhint %}

## 若到期、事件量超出限额、扩展节点但不更新 License ，会有什么影响？

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53EF;&#x80FD;&#x7684;&#x60C5;&#x51B5;</th>
      <th style="text-align:left">&#x5F71;&#x54CD;</th>
      <th style="text-align:left">&#x751F;&#x6548;&#x7248;&#x672C;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x9879;&#x76EE;&#x6570;&#x6EE1;&#x989D;</td>
      <td style="text-align:left">&#x53EF;&#x6B63;&#x5E38;&#x4F7F;&#x7528;&#x7CFB;&#x7EDF;&#xFF0C;&#x65E0;&#x6CD5;&#x521B;&#x5EFA;&#x65B0;&#x7684;&#x9879;&#x76EE;</td>
      <td
      style="text-align:left">4.2.2 &#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#x65F6;&#x95F4;&#x5230;&#x671F;</td>
      <td style="text-align:left">&#x65E0;&#x6CD5;&#x4F7F;&#x7528;&#x7CFB;&#x7EDF;</td>
      <td style="text-align:left">4.2.2 &#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#x4E8B;&#x4EF6;&#x91CF;&#x8D85;&#x51FA;&#x9650;&#x989D;</td>
      <td style="text-align:left">
        <p>&#x5728;&#x6743;&#x9650;&#x65F6;&#x95F4;&#x5185;&#xFF0C;&#x8282;&#x70B9;&#x6570;&#x672A;&#x8D85;&#x7684;&#x60C5;&#x51B5;&#x4E0B;&#x53EF;&#x4EE5;&#x7EE7;&#x7EED;&#x4F7F;&#x7528;&#x7CFB;&#x7EDF;&#xFF0C;</p>
        <p>&#x4F46;&#x4F1A;&#x81EA;&#x52A8;&#x5173;&#x95ED;&#x6240;&#x6709;&#x9879;&#x76EE;&#x6570;&#x636E;&#x6D41;&#xFF0C;</p>
        <p>&#x8D85;&#x51FA;&#x7CFB;&#x7EDF;&#x914D;&#x7F6E;&#x7684;&#x65F6;&#x95F4;&#x540E;&#x4F1A;&#x5BFC;&#x81F4;&#x65B0;&#x7684;&#x6570;&#x636E;&#x4E22;&#x5931;</p>
      </td>
      <td style="text-align:left">4.6&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#x8282;&#x70B9;&#x6570;&#x8D85;&#x51FA;&#x9650;&#x989D;</td>
      <td style="text-align:left">&#x65E0;&#x6CD5;&#x4F7F;&#x7528;&#x7CFB;&#x7EDF;</td>
      <td style="text-align:left">4.3.4&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#x66F4;&#x6362;&#x4E86;&#x6838;&#x5FC3;&#x8C03;&#x5EA6;&#x8282;&#x70B9;</td>
      <td
      style="text-align:left">&#x65E0;&#x6CD5;&#x4F7F;&#x7528;&#x7CFB;&#x7EDF;</td>
        <td style="text-align:left">4.6&#x53CA;&#x4EE5;&#x4E0A;</td>
    </tr>
  </tbody>
</table>## 为什么我的License无效？

License中会包含机器码，如果与部署环境的机器码不一致，则License无效，所以请务必使用当前系统的机器码来生成License。

## 从哪里获取机器码？

1. 企业概览-当前计划-复制机器码 
2. 激活/到期/节点变更/机器码变更弹窗中复制机器码

{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

