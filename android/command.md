### 调试相关
***

#### apk相关操作

``` shell
#查看所有安装的包
#可以添加参数 例如-f:可以打印路径名 -3:打印第三方apk
adb shell pm list packages
#查看apk路径 是apk文件
pm path packages.name #example:pm path com.android.providers.media
#dumpsys apk相关信息 可能能获取到apk名字
adb shell dumpsys package packages.name
```

#### 查看当前Activity

Activity相关配置通过AndroidManifest.xml Android Studio可以直接打开查看(对应apk或者源码)

``` shell
#当前运行activity
adb shell dumpsys activity top | grep ACTIVITY
#当前运行的app
adb shell dumpsys window | grep  mCurrentFocus
```
