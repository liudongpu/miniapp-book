### 集成指南

项目根目录 build.gradle的allprojects的repositories添加子项
```gradle
maven {
            // All of React Native (JS, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
```


app目录下的build.gradle添加引用
```gradle
implementation "com.facebook.react:react-native:0.59.10" // From node_modules
```

添加task 自动判断下载文件
```gradle
task miniappInit(){
    if(file("$rootDir/../node_modules/versions/0.59.10.md").exists()){

        println("miniappInit skip check")
    }
    else{
        println("miniappInit clone soure")
        exec {
            commandLine "git","clone","--branch","v-0.59.10","https://code.aliyun.com/uhutu-miniapp/miniapp-libs.git","$rootDir/../node_modules/"
        }
    }
};

afterEvaluate {
    tasks.matching {
        it.name.equals('preBuild')
    }.each { tk ->
        tk.dependsOn(miniappInit);
    }
}


```