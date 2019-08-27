---
id: event
title: 事件通知
---

## 跨页面通知
this.pageEventRegister
### 参数描述
|参数名称|参数类型|示例值|描述|
|:---|:---:|---:|---:|
|sEventName|string|demo|通知的名称|
|fListener|Function|(data:T)=>void|通知触发的函数|
### 响应结果
|参数名称|参数类型|示例值|描述|
|:---|:---:|---:|---:|
### 示例
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

### 备注信息
参考页面：demo/one/tool 和 demo/one/url




## 原生发送信息

### 小应用页面
在页面初始化时调用：
```ts
/**
 * 定义通知信息的结构
 */
interface INoticeInfo extends IBaseNativeNotice {

}
class Demo {


  test(){
    /**
     * 这个注册监听原生的通知
     */
    UtilsSuperBridge.getInstance().noticeAddLisenter("notice_abc", this.noticeEvent);
  }


  //这个是触发事件
  noticeEvent(oInfo: INoticeInfo) {
    UtilsSuperCommon.logWarn(oInfo);
  }

  /**
   * 在页面卸载时必须反注册掉事件的名称
   */
  subPageUnmount() {
    UtilsSuperBridge.getInstance().noticeRemoveLisenter("notice_abc");
  }


} 
 ```
### android发送通知
```java
        Map<String,String> map=new HashMap<>();
        map.put("aaa","bbb");

        new MiniappJumpUtil().sendNativeNotice("notice_abc",map);
```

### ios发送通知
```objective-c 
MiniappJumpUtil *bridge=[MiniappJumpUtil new];
    
    NSMutableDictionary *dic= [[NSMutableDictionary alloc] init] ;
    dic[@"aa"] = @"bb";
    [bridge sendNativeNotice:@"notice_abc" withDic:dic];
```