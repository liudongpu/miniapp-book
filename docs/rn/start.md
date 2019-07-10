---
id: start
title: 小应用集成指南
---

## 开始步骤  
* 安装nodejs环境
* 执行 `npm install --registry=https://registry.npm.taobao.org` ，更新依赖包
* `npm run local` 初始化本地特殊操作 必须执行
* `sudo npm run install-cli` 安装命令行辅助程序，可选项。安装完成后执行`icome-cli -h`参考命令行的辅助说明
* `npm run dev`，启动开发环境



## 开发步骤
* 首先参考src/pages/demo/one 下的文件夹
* app.tsx 入口启动文件，里面配置导航的路径，页面的引用
* appconfig.json 小应用的配置选项，具体参数描述参考ppt
* *.tsx  编写具体的页面文件 
* 然后修改/index.js文件，将`import App from './src/pages/demo/one/app';`改为自己调试的路径
* `npm run dev`，启动开发环境

## icome-cli辅助命令
* 执行`icome-cli -h`查看对应的命令及说明，以下是部分示例命令
* "-m"为"--miniapp"的缩写，表示小应用id;"-p"为"--page"的缩写,表示页面名称
* `icome-cli init -m demo_one` 初始化文件路径为src/pages/demo/one的小应用
* `icome-cli refresh -m demo_one` 刷新小应用的路由信息
* `icome-cli add -m demo_one -p example` 在demo/one下添加一个example.tsx页面
* `icome-cli version -m demo_one` 更新demo_one的code和版本号



## 命名规范 

* 文件夹/文件名保持英文小写，无特殊符号
* pages下的.tsx页面代码，全部`export default class extends 基类`，不指定具体class名称  
* services下的.ts逻辑代码，全部`export class 类名称`,不指定defult，原则上方法全部static，其中有些接口可以导出  
* themes下的.ts样式定义，`export default StyleSheet.create<IAirThemeStyle>({})`，这种写法定义  
* 类名称所有单词第一个字母大写，根目录子目录文件名，一个.ts文件通常只有一个类。
* 方法名称命名，第一个单词小写开头后面的单词大写开头
* 私有的以private标记上或者以_开头
* that作为保留关键字，一般用于方法的第一个参数，由页面传递参数过来
* page子类禁止调用生命周期相关函数，参见页面生命周期函数定义
* 注意，如果一个页面中有icome_auto_sign_begin到icome_auto_sign_end这种注释风格，begin到end中的内容不要手动修改，一般用于自动生成的

## 调试
* app.tsx中的createStackNavigator第二参数initialRouteName属性可以指定调试首页，但是提交发布时一定要改回去  
* 下载对应的调试专用app，然后打开小应用，摇一摇，在Dev  Settings中点击`Debug server host & port for device`
* 输入本机电脑的ip:8081，示例`10.0.1.1:8081`，然后reload即可


## 页面生命周期函数
* 原则上React的默认的生命周期函数禁止调用 component系列 render系列都禁止使用，继承基类的使用
* `subPageInit(props:P)` 在类初始化时调用，参数为属性值，在这里可以初始化state,写法为`this.state={a:'1'}`。除了此方法外更改state需要调用`this.setSate({a:'2'});`这种写法更新state
* `subPageLoad()` 这个是页面加载时调用，如果有请求api之类的，在这个方法里使用
* `subPageUnmount()` 页面卸载时调用方法，一般用于注销监听，销毁对象，销毁定时之类的操作，基本用不上
* `subPageRender(): ReactNode` 这个是抽象方法，每个页面都需要提供，返回的是页面展示对象


## 关键类描述
* UtilsSuperBridge  这个是与原生交互的类
* UtilsSuperApi  这个是调用API接口的操作类
* UtilsSuperGuide  这个是通用常规操作类


## 继承关系
* UtilsRootApp  所有app.tsx核心路由配置页面继承此类
* UtilsRootPage  所有.tsx的功能页面都继承此类  实现subPageRender方法


## 其他命令
* `npm run build-alhpa` 开发环境打包代码，会执行 打包、压缩、更新版本号 等操作
* `npm run server` 本地调试文件服务器
* `npm run typedoc` 生成注释文件页面 
* `npm run plus-cli` 编译plus的cli文件
* `npm run deploy-beta` 发布beta的更新
* `npm run deploy-preview` 发布preview的更新
* `npm run deploy-release` 发布release的更新


## 项目文件夹目录

> *.vscode*  VSCode的配置项 
> 
> *build*   打包生成的目录
>> *bundle* 生成的对应平台的代码  
>> *zip* 压缩包  
>> *version* 版本升级文件夹  
>
> *config*   配置文件夹
>> *appversion.json* 升级的配置文件，管理各种环境的配置版本   
>  
> *plus*   功能扩展文件夹 
>
> *src*    源代码文件夹  
>> *pages* 页面相关目录  
>> *rs* 自定义ui组件，起名源于android的R  
>> *services* 业务逻辑代码,原则上结构为group/feature.ts,自定义接口也存于此文件夹中  
>> *themes* 样式定义  
>> *utils* 公用代码文件   
>>> *define* 定义文件   
>>> *face* 通用接口文件   
>>> *inc* 配置相关   
>>> *root* 通用基类继承   页面和app都需要继承此文件夹下的基类    
>>> *super* 通用方法 原则上单例 
>>> *widget* 一些常用的页面的基类定义  
>
> *app.json* 基本定义，勿动  
> *App.tsx* 默认示例，无用，留着测试  
> *gulpfile.js* 各种任务的配置  
> *index.js* 启动配置项，这里定义调用哪个小应用
> *package.json*    npm配置文件  
> *rn-cli.config.js*  RN的命令行配置项  
> *tsconfig.json*  typescript的定义文件  
> *logs* 日志文件夹  

## 发布说明
* code规范：8位数字组成，年月日(例如：180727)+当日更新次数(两位数字：01)  例如:18072701  
* 发布环境默认发布到阿里云oss，注意本地执行没有对应的授权配置不能发布。只有在build/key下有miniapp.json文件，才有权限发布到阿里云
* config/appversion.json 这个文件管理对应的各个环境的发布配置，注意需要更新此文件才会更新对应环境的发布项，为了安全，这个文件目前手动维护
* versions下找到对应小应用的对应环境下的版本编码，修改为要发布的8位数字版本编码，然后提交，然后执行jenkins上的对应任务。
* 如果需要手动发布，将build下生成的对应环境的小应用的配置文件发给管理员，人工上传到OSS即可。


## api调用
* api调用原则上放在services/api目录下
* 每个后台接口定义一个方法,使用Promise风格的返回参数
* 后台返回的json数据默认会有code,message,data三个对象，只需要定义data的类型结构，并且默认调用的UtilsSuperApi.getInstance().apiPost()方法也只返回的是data对象
* 如果需要完成的结构体，使用UtilsSuperApi.getInstance().rootApiPost(),原则上不要使用这个底层方法。







## 参考地址

* [react-native官网](https://facebook.github.io/react-native/)
* [react-native中文](https://reactnative.cn/)
* [native-base组件](https://docs.nativebase.io/)
* [vetor-icons Github代码](https://github.com/oblador/react-native-vector-icons)
* [vector-icons图标库](https://oblador.github.io/react-native-vector-icons/)
* [react-navigation导航配置](https://reactnavigation.org/zh-Hans/)