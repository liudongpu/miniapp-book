---
id: event
title: 跨页面通知
---

## 调用方法
this.pageEventRegister
## 参数描述
|参数名称|参数类型|示例值|描述|
|:---|:---:|---:|---:|
|sEventName|string|demo|通知的名称|
|fListener|Function|(data:T)=>void|通知触发的函数|
## 响应结果
|参数名称|参数类型|示例值|描述|
|:---|:---:|---:|---:|
## 示例
页面A注册通知
```ts
//定义通知的参数结构
interface IEventData extends IBaseEventData{
  demoOne:string
}


subPageInit(){
    //...
    this.pageEventRegister("eventone",(data:IEventData)=>{
      //这里写该事件触发时的操作
    })
}
```

页面B触发通知
```ts
UtilsSuperGuide.getInstance().eventPageEmit("eventone",{demoOne:"子页面威武"})
```

## 备注信息
参考页面：demo/one/tool 和 demo/one/url