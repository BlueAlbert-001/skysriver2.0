# .getFlowConfig

## 概述

请先完成阅读：[API方式接入广告位](./)

{% hint style="warning" %}
使用该接口前，请先进行[SDK初始化](../initialization.md)
{% endhint %}

`getFlowConfig`接口用于获取广告位id对应配置详情，调用该接口后，请根据接口的返回值，采用对应的方式处理。

#### 创意ID的获取

调用了getFlowConfig后，返回的creativeId的值即为创意ID

## **调用方法**

```java
wx.tmSDK.getFlowConfig({
     positionId: positionId
}).then((config) => {
    console.log('该广告位当前配置', config);
});
```

## **传入参数**

positionId将对应的不同类型返回值，使用前请确认当前使用的positionId类型

| 字段 | 字段类型 | 说明 |
| :--- | :--- | :--- |
| positionId | string | 广告位id |

## **返回值与广告类型**

### **浮动窗广告位**

* type为1时，positionId对应的是浮动窗类型
* 如果创意列表creatives的show\_config中，只有image，则只需渲染该静态图
* 浮动窗广告可在天幕设置角标功能

```java
{
    "isOpen":true,
    "type": 1, // 浮动icon类型
    "auto_change":10,
    "cornerOpen": true, // 角标是否开启
    "borderStyle": { // 当天幕后台配置了边框时，此处会出现边框样式属性
        "imageUrl": "https://cdn.kuaiyugo.com/plat/SDK/sdk_img_res/border3_title.png",
        "left": 32, // show_config中图片相对边框的左位置
        "size": "254x368", // 边框尺寸，通常比show_config中的图片大
        "top": 66 // show_config中图片相对边框的上位置
    },
    "creatives":[
        {
            "creativeId":162,
            "positionId":"1013280",
            "show_config":{
                "image": "https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_6fe2c240-e1a7-11e8-bc47-27d9eee1c822.png",
                "show_type":0 // 动态效果
            },
            "size":"190x270", // 广告位尺寸比例
            "corner_style": { // 角标的样式
                "left_top": { // 左上角
                    "type": 1, // 1:红点，2:角标
                    "image": "https://cdn.kuaiyugo.com/plat/SDK/test/static_files/corner_icon/200200/red_dot/left_top/red_dot_2.png",
                    "width": 40,
                    "height": 40
                },
                "left_bottom": { // 左下角
                    "type": 2,
                    "image": "https://cdn.kuaiyugo.com/plat/SDK/test/static_files/corner_icon/200200/red_dot/left_bottom/red_dot_1.png",
                    "width": 75,
                    "height": 75
                },
                "right_top": {}, // 右上角
                "right_bottom": {} // 右下角
            }
        }
    ]
}
```

* 如果创意列表creatives中，有fps，多个图片素材则需渲染为动态图片，按照fps切换每张图片
* 示例：fps=5，意味着每秒播放5张图片，即0.2秒切换一张

```java
{
    "isOpen":true,
    "type": 1,
    "auto_change":10,
    "creatives":[
        {
            "creativeId":162,
            "positionId":"1013280",
            "show_config":{
                "fps":0.5,
                "images":["https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_6fe2c240-e1a7-11e8-bc47-27d9eee1c822.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_7296e5c0-e1a7-11e8-9e7e-c3d1548f6807.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_74d033f0-e1a7-11e8-bc47-27d9eee1c822.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_76f7a7d0-e1a7-11e8-9e7e-c3d1548f6807.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_794679d0-e1a7-11e8-bc47-27d9eee1c822.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_7ca4c000-e1a7-11e8-9e7e-c3d1548f6807.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_7ebeed70-e1a7-11e8-bc47-27d9eee1c822.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_80b650a0-e1a7-11e8-9e7e-c3d1548f6807.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_8b929e70-e1a7-11e8-bc47-27d9eee1c822.png","https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_985deb00-e1a7-11e8-9e7e-c3d1548f6807.png"],
                "show_type":0
            },
            "size":"190x270"
        }
    ]
}
```

### 多icon广告位

{% hint style="info" %}
更新说明：此类型广告位，在2020/04/02后，支持多帧动图（返回多张图片，按照指定速率轮播，实现gif效果），如需使用，请参照下文做对应兼容
{% endhint %}

* type为7时，positionId对应的是多icon广告类型
* 多icon广告位拥有多个创意，需要全部渲染处理
* 多icon广告位可在天幕设置角标功能

```java
{
    "isOpen":true,
    "type": 7, // 多icon广告类型
    "cornerOpen": true, // 角标是否开启
    "creatives":[
        {
            "creativeId":162,
            "positionId":"1013280",
            "show_config":{
                "image": "https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_6fe2c240-e1a7-11e8-bc47-27d9eee1c822.png",
                "title": '游戏名' //游戏名称 
            },
            "corner_style": { // 角标的样式
                "left_top": { // 左上角
                    "type": 1, // 1:红点，2:角标
                    "image": "https://cdn.kuaiyugo.com/plat/SDK/test/static_files/corner_icon/200200/red_dot/left_top/red_dot_2.png",
                    "width": 40,
                    "height": 40
                },
                "left_bottom": { // 左下角
                    "type": 2,
                    "image": "https://cdn.kuaiyugo.com/plat/SDK/test/static_files/corner_icon/200200/red_dot/left_bottom/red_dot_1.png",
                    "width": 75,
                    "height": 75
                },
                "right_top": {}, // 右上角
                "right_bottom": {} // 右下角
            }
        },
         {
            "creativeId":162,
            "positionId":"1013280",
            "show_config":{
                "image": "https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_6fe2c240-e1a7-11e8-bc47-27d9eee1c822.png",
                "title": '游戏名' //游戏名称 
            },
        }
    ]
}
```

* 如果创意列表creatives中，**有fps**，且**返回多张图片**（images为list，示例如下），则多个图片素材需渲染为动态图片——按照fps循环渲染每张图片
* 示例：**fps=5，意味着每秒播放5张图片，即0.2秒切换一张（并不是每5s切换一张图片！！）**
* 通过上述方式，在游戏中实现类似gif动图的效果
* 同时接口返回中仍会有image（单张图片）返回（此为避免部分老游戏游戏SDK直接更新&&未做此部分处理，可能导致游戏异常而做的保险机制），**可忽略**。（SDK2.6.9-2020-04-22更新）
* 关于点击后的跳转：即使是多张图片轮播，仍旧是归属于同一个创意（creativeID），也就是说，跳转的时候，只会跳转到同一个游戏。参考跳转接口：.[flowNavigate](landing.md)

```javascript
{
    "creatives":[
        {                    
            "creativeId":162,
            "positionId":"1013280",
            "show_config":{
                //多张图片拼接成动态图片
                "images":[
                    "https://cdn.kuaiyugo.com/test/tianmu/2020-03-20_e16178806a5411eaa39ff9aa2d973067.png",
                    "https://cdn.kuaiyugo.com/test/tianmu/2020-03-20_e545dea06a5411eaa992514bb3382df2.png"
                ],
                "fps":5,
                "title":'游戏名',
                 //同时依旧会返回image，可忽略
                "image": "https://cdn.kuaiyugo.com/appprogram/tiger/admin/2018-11-06_6fe2c240-e1a7-11e8-bc47-27d9eee1c822.png"
              },
        },
    ]  
}
```

### Banner广告位

* type为11时，positionId对应的是Banner类型

```javascript
{
    "isOpen":true,
    "type": 11, // Banner类型
    "creatives":[
        {
            "creativeId":162,
            "positionId":"1013280",
            "show_config":{
                "image": "https://cdn.kuaiyugo.com/test/tianmu/2019-06-26_959a525097be11e9aee7bfaee0856d23.jpg"
            }
    ]
}
```

### **返回参数**

| 字段 | 字段类型 | 说明 |
| :--- | :--- | :--- |
| isOpen | boolean | 广告位是否开启 |
| type | string | 该广告位的类型 |
| auto\_change | number | 自动切换时间 |
| borderStyle | object | 边框样式属性  \*仅当后台配置开启时 |
| borderStyle.imageUrl | string | 边框图片 |
| borderStyle.size | string | 边框尺寸，通常比show\_config中的图片大 |
| borderStyle.left | number | show\_config中图片相对边框的左位置 |
| borderStyle.top | number | show\_config中图片相对边框的上位置 |
| creatives | array | 该广告位可渲染的创意列表, 浮动icon类型该数组有且只有一个 |
| creativeId | string | 创意id |
| positionId | string | 广告位id |
| show\_config | object | 广告位素材详情 |
| show\_config.title | string | 素材对应渲染的标题 |
| show\_config.image | string | 素材图片 |
| title | string | 多icon广告位的广告文案 |
| cornerOpen | boolean | 角标是否开启 |



