# **目录**

 [initSDK](#a0)

 [showItemByUrl](#a1)

[开普勒模块升级到京东联盟模块](#a2)

# 准备

1. 进入京东联盟（https://union.jd.com/）
2. 注册登录，我的推广----〉推广管理----〉APP管理，创建Android与iOS应用
3. 审核过后应用的「操作」那点击查看，系统会根据实际bundleid与安卓的apk包来生成专有的SDK，并下载SDK
4. 下载并解压京东联盟 安全图片模块。[点击下载]()
5. 解压  **步骤4**  所下载的iOS SDK，复制 **Kepler.bundle** 到 **步骤5** 中的以下路径替换同名文件``/mJDUnionPic_iOS/mJDUnionPic/target``
6. 解压  **步骤4**  所下载的Android SDK，复制``/src/main/res/raw``下的 **safe.jpg** 到 **步骤5** 中的以下路径替换同名文件``/mJDUnionPic_Android/mJDUnionPic/res_mJDUnionPic/res/raw``
7. 注意安卓下还需要修改验证模块中的``AndroidManifest.xml``中的值
8. 分别压缩 **步骤6** 与 **步骤7**各自的mJDUnionPic目录为zip包
9. 打开apicloud控制台，并选择你的应用
10. 依次点击：左侧菜单【模块】—>右侧顶部【自定义模块】
11. 模块名称填入mJDUnionPic，并分别上传 **步骤8** 生成的zip包，注意对应
12. 保存模块，并添加mJDUnionPic模块
13. **注意：本模块不可以与京东开普勒模块同时使用**

#  配置

## 需要在config.xml配置如下信息，并上传编译来生效

```xml
<preference name="querySchemes" value="tmall,tbopen,jdlogin,openapp.jdmobile" />
<feature name="mJDUnion">
    <param name="urlScheme" value="sdkback+appKey" />
    <param name="appKey_iOS" value="appKey" />
    <param name="appSecret_iOS" value="appSecret" />
    <param name="appKey_Android" value="appKey" />
    <param name="appSecret_Android" value="appSecret" />
</feature>
```

<div id="a0"></div>

# **initSDK**

初始化京东联盟SDK
initSDK(callback(ret, err))

## callback(ret, err)

ret：

- 类型：JSON对象
- 内部字段：

```js
{
    code: '0',
    message: 'success'
}
```

err：

- 类型：JSON对象
- 内部字段：

```js
{
    code: '1000',                   //错误码
    message: 'user is not exist'      //错误描述
}
```

## 示例代码

```js
var mJDUnion = api.require('mJDUnion');
mJDUnion.initSDK(function(ret, err) {
    if (ret) {
        alert('success：' + JSON.stringify(ret));
    } else {
        alert('error：' + JSON.stringify(err));
    }
});
```

## 可用性

iOS系统，Android系统
可提供的1.0.0及更高版本

<div id="a1"></div>

# **showItemByUrl**

通过URL打开任意商品页面
showItemByUrl({params},callback(ret, err))

## params

url:

- 类型：字符串
- 描述：商品的页面url，如 https://item.jd.com/3791763.html

customParams:

- 类型：JSON 对象
- 描述：（可选项）传参数据为第三方应用自定义,可以为页面,频道标识;也可以标识分成信息;该数据只做统计需求。（不建议传入中文以及特殊字符） **\* 禁止传参带入以下符号：   =#%&+?<{}**，只支持iOS

actId:

- 类型：字符串
- 描述：（可选项）设置ActId 京东达人内容ID，只支持iOS

ext:

- 类型：字符串
- 描述：（可选项）内容渠道扩展字段，只支持iOS

virtualAppkey:

- 类型：字符串
- 描述：（可选项）计费到另外一个账号体系的appkey（为空或不传则计入到当前SDK申请的账号下），只支持iOS

## callback(ret, err)

ret：

- 类型：JSON对象
- 内部字段：

```js
{
    code: '0',
    message: 'success'
}
```

err：

- 类型：JSON对象
- 内部字段：

```js
{
    code: '1000',                   //错误码，详见京东那边的错误码，如果未安装京东app的或者唤端失败的在这里返回
    message: 'user is not exist'      //错误描述
}
```

## 示例代码

```js
var mJDUnion = api.require('mJDUnion');
var param = {
    url : 'https://item.jd.com/3791763.html',
    customParams: {}
};
mJDUnion.showItemByUrl(param);
```

## 可用性

iOS系统，Android系统
可提供的1.0.0及更高版本

<div id="a2"></div>

# **开普勒模块升级到京东联盟模块（不是很推荐）**

1. 查找`api.require('mKepler')`替换为`api.require('mJDUnion')`
2. 删除除了initSDK与showItemByUrl之外的接口
3. showItemByUrl增加了返回参数，如果没有安装京东app，不会像开普勒一样用开普勒内置Webview打开，需要自行处理