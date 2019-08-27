---
id: route
title: 路由及跳转
---

## 一、页面跳转
使用场景：小应用内页面之间跳转   
注意 页面地址需要在"app.tsx"页面的路由注册中添加后才能跳转
### 1.1 继承RootPage
`UtilsSuperGuide.getInstance().navigationUrl(that: IBaseRootPage, sUrl: string)`
### 1.2 常规组件
`UtilsSuperGuide.getInstance().navigationJump(sUrl: string)`


## 二、初始化跳转页面
使用场景：app中指定小应用的默认起始页
### 2.1 常规链接协议
`icome-miniapp://demo_one.app`
### 2.2 指定默认起始页
假设起始页的页面名为`url`并且传入参数值`field_a1=字段一号&field_a2=b`  
* 拼接协议  
拼接为标准格式:`url?field_a1=字段一号&field_a2=b`  
* 链接编码
进行urlencode后为：  
`url%3ffield_a1%3d%e5%ad%97%e6%ae%b5%e4%b8%80%e5%8f%b7%26field_a2%3db`
* 附加参数
将编码后的路径参数附加为"system_uec-init_router"的值  
`icome-miniapp://demo_one.app?system_uec-init_router=url%3ffield_a1%3d%e5%ad%97%e6%ae%b5%e4%b8%80%e5%8f%b7%26field_a2%3db`

## 三、获取链接参数
使用场景：小应用的页面中获取默认链接传递的参数  
### 3.1 使用示例
假设链接为`icome-miniapp://demo_one.app?f_a=123&f_b=456`
页面中要取到链接中的参数，使用方式如下：
```ts

/**
 * 定义路由的参数格式
 */
export interface IBaseRouterInit{
    f_a:string
    f_b:string
}

//以下是调用地方
  
  let oUrlInfo = UtilsSuperCommon.upUrlInfo(UtilsIncConfig.getInstance().upInitappInfo().envUrl);
  let oRouterInit:IBaseRouterInit = UtilsSuperCommon.convertSchema<IBaseRouterInit>(oUrlInfo.queryObject);

  consoloe.log(oRouterInit.f_a);
```
### 3.2 保留规则
字段名中禁止使用中划线"-"，可以使用下划线"_"。中划线作为特殊标记规则格式化使用。
例如"system_uec-"标记为该字段使用的是"urlencode"规则，会将后面的字段名所对应的值自动解码"decodeURIComponent"

