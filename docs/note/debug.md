---
id: debug
title: 调试配置
---

## 版本选择


## 调试环境切换
在某些时候，需要切换本地调试代码的环境变量，需要做如下调整
index.js文件中
```ts
import {AppRegistry} from 'react-native';
import App from './src/pages/demo/one/app';

//增加如下两行代码 指定本地调试代码的环境变量
import {UtilsIncConfig}  from './src/utils/inc/config';
UtilsIncConfig.getInstance().inInitappConfig({envName:"beta"});

AppRegistry.registerComponent('MiniappPoject', () => App);
```

### 环境描述
envName取值范围： 
|值|描述|
|:---|:---:|
|alpha|开发环境(默认值)|
|beta|测试环境|
|preview|预生产环境|
|release|正式环境|



## 奇技淫巧
### ios真机调试配置
由于ios真机无法自动获取主机ip，调试时需指定主机ip请求
```oc
[miniJump jumpUrl:@"debug-miniapp://http://10.4.143.141:8081/index.bundle?platform=ios" withView:self ];
```

### 调试指定页面
适用场景：如果一个子页面需要多次点击操作才能进入，可以调试时指定默认首页
oc
```oc
[miniJump jumpUrl:@"debug-miniapp://?system_uec-init_router=url" withView:self ];
```
android
```android
MiniappSupport.getInstance().jumpUrl("debug-miniapp://?system_uec-init_router=url",miniappDelegate,getApplicationContext());
```
如果需要跳转带参数，参考编码系列