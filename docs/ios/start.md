---
id: start
title: iOS集成方式
---

## 集成react-native

项目根目录 podfile 添加子项
```ruby
#begin 以下是react-native相关依赖#
  
  flagExist=File::exists?( "../node_modules/versions/0.59.10.md" )
  if !flagExist
      system 'rm -rf ../node_modules/'
      system 'git clone --branch v-0.59.10 https://code.aliyun.com/uhutu-miniapp/miniapp-libs.git ../node_modules/'
  end
  # Your 'node_modules' directory is probably in the root of your project,
  # but if not, adjust the `:path` accordingly
  pod 'React', :path => '../node_modules/react-native', :subspecs => [
  'Core',
  'CxxBridge', # Include this for RN >= 0.47
  'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
  'RCTText',
  'RCTNetwork',
  'RCTImage',
  'RCTLinkingIOS',
  'RCTWebSocket', # Needed for debugging
  'RCTAnimation', # Needed for FlatList and animations running on native UI thread
  # Add any other subspecs you want to use in your project
  ]
  # Explicitly include Yoga if you are using RN >= 0.42.0
  pod 'yoga', :path => '../node_modules/react-native/ReactCommon/yoga'
  
  # Third party deps podspec link
  pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
  pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
  pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'
  
  
  pod 'RNVectorIcons', :path => '../node_modules/react-native-vector-icons'
  
  #end react-nativei相关依赖完成#
```


## 集成miniapp-ios

项目根目录 podfile 添加子项

```ruby
source 'https://github.com/liudongpu/miniapp-specs.git'

#begin 以下是miniapp相关依赖#
  pod 'MiniappIos',  '~> 0.0.3'
#end miniapp相关依赖完成#

```

## 调整项目配置

调整 Info.plist 
### *按需添加需要的字体*
添加属性`Fonts provided by application` (或者如果xcode不支持,使用 UIAppFonts),添加子属性值，
 ```xml
<array>
	<string>MaterialIcons.ttf</string>
	<string>SimpleLineIcons.ttf</string>
	<string>EvilIcons.ttf</string>
	<string>Zocial.ttf</string>
	<string>FontAwesome.ttf</string>
	<string>Ionicons.ttf</string>
	<string>Foundation.ttf</string>
	<string>Feather.ttf</string>
	<string>Octicons.ttf</string>
	<string>MaterialCommunityIcons.ttf</string>
	<string>FontAwesome.ttf</string>
</array>
 ```
### *状态栏调整*
`View controller-based status bar appearance`属性值设置为no

### *调整ats*
Apple has blocked implicit cleartext HTTP resource loading. So we need to add the following our project's Info.plist (or equivalent) file.
```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>localhost</key>
        <dict>
            <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
            <true/>
        </dict>
    </dict>
</dict>
```