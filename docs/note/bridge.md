---
id: bridge
title: 桥接模型
---
## 桥接说明
ios和android都实现统一的MiniappManagerBridge类，注册进react-native的method并导出。  
有两种场景，一种模型为通知事件
## 桥接分类
+ sendNativeEvent [通知事件](#事件模型-通知事件)  
+ sendNativePromise [回调事件](#事件模型-回调事件)  



## 事件模型-通知事件
ios和android都实现统一的MiniappManagerBridge类，注册进react-native的method并导出。  
有两种场景，一种模型为通知事件

## 事件模型-回调事件
回调会有返回参数。


## 参照实现代码
```js

import { NativeModules } from 'react-native';
import { ISystemManagerBridge, ISystemNativeEvent } from '../face/system';
var mangerBridge: ISystemManagerBridge = NativeModules.MiniappManagerBridge;

/**
 * 用户信息接口
 */
interface IUserToken {
    token: string
}

/**
 * 设备信息接口
 */
interface IDeviceInfo {
    /**
     * 系统类型
     */
    systemType: string
    /**
     * 系统版本号
     */
    systemVersion: string
    /**
     * 系统类型
     */
    systemModel: string
    /**
     * 厂家名称
     */
    deviceBrand: string
}



export class UtilsSuperBridge {


    private static instance = new UtilsSuperBridge();
    static getInstance() {
        return this.instance;
    }


    /**
     * 核心调用原生事件无返回值
     * @param sEventType 
     * @param oEventParam 
     */
    private sendNativeEvent(sEventType: string, oEventParam: ISystemNativeEvent) {


        oEventParam.eventType = sEventType;

        mangerBridge.sendNativeEvent(sEventType, JSON.stringify(oEventParam), "{}");
    }


    /**
     * 核心调用原生方法并且返回Promise对象值
     * @param sEventType 
     * @param oEventParam 
     */
    private sendNativePromise<T>(sEventType: string, oEventParam: ISystemNativeEvent): Promise<T> {

        return mangerBridge.sendNativePromise<string>(sEventType, JSON.stringify(oEventParam), "{}").then((sJson: string) => { return JSON.parse(sJson); });

    }




    /**
     * 获取用户token信息
     */
    nativePromiseToken() {
        return this.sendNativePromise<IUserToken>('nativePromiseToken', {});
    }

    /**
     * 获取设备信息
     */
    nativePromiseInfo() {
        return this.sendNativePromise<IDeviceInfo>('nativePromiseInfo', {});
    }



    /**
     * 调用原生返回上一视图操作  注意，这个操作会退出整个小应用返回之前的页面
     */
    nativeEventBack() {
        this.sendNativeEvent('nativeEventBack', {});
    }



    /**
    * 调用原生跳转链接操作
    */
    nativeEventJump(sUrl: string) {
        this.sendNativeEvent('nativeEventJump', { targetUrl: sUrl });
    }


    /**
    * 调用原生Toast
    */
    nativeEventToast(sMessage: string) {
        this.sendNativeEvent('nativeEventToast', { messageInfo: sMessage });
    }


    /**
    * 调用原生隐藏加载中
    */
    nativeEventLoadClose() {
        this.sendNativeEvent('nativeEventLoadClose', {});
    }


    

    /**
    * 调用原生隐藏按钮
    */
    nativeEventHidenav() {
        this.sendNativeEvent('nativeEventHidenav', {});
    }





}
```