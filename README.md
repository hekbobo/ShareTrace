

# Android集成文档
## 一、下载导入最新版SDK
前往下载页下载最新版的Android SDK 。前往下载

从下载的 ShareTraceSDK 压缩包中，将 sharetrace 的 aar 文件复制到app module目录下的libs文件夹中， 然后打开app module目录下的build.gradle配置文件，在android一栏中添加依赖：


```
repositories {
    flatDir {
        dirs 'libs'
    }
}
```





然后再在dependencies一栏中添加：


implementation(name: 'sharetrace-android-sdk_vX.X.X', ext: 'aar')

## 二、配置APP_KEY
在 AndroidManifest.xml 文件中的 application 标签内设置 AppKey
```
<meta-data
	android:name="com.sharetrace.APP_KEY"
	android:value="ShareTrace AppKey"/>
请将 ShareTrace AppKey 替换成 sharetrace 为应用分配的 appkey
```

## 三、初始化ShareTrace
在自定义的Application的onCreate()中初始化ShareTrace。 如果没有自定义Application可以参考下面示例代码自定义Application。

示例代码

在Application中调用初始化代码。
```
public class MyApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        ShareTrace.init(this);
    }
}
```
在AndroidManifest.xml中的application标签中添加android:name=".MyApp" 指定自定义的Application类。

```
<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:name=".MyApp"
    android:theme="@style/AppTheme">
    ...
</application>
```

## 四、获取安装携带的参数
当 APP 需要获取安装参数时（由 web 网页中传递过来的，如邀请码、用户id、来源网页等动态参数），调用ShareTrace.getInstallTrace方法，在回调方法中获取相应参数。

```
ShareTrace.getInstallTrace(new ShareTraceInstallListener() {
    @Override
    public void onInstall(AppData data) {
        Log.i(TAG, "appData=" + data.toString());
    }

    @Override
    public void onError(int code, String msg) {
        Log.e(TAG, "Get install trace info error. code=" + code + ",msg=" + msg);
    }
});
```
注意： 该方法可以重复调用，请处理好反复调用的逻辑。

## 五、上传安装包
将集成好的应用导出APK安装包上传。 注意 **不能是DEBUG版本，因为DEBUG版本会被标记为testOnly，只能通过adb进行安装；APK打包需要选择V2签名，不支持未使用V2签名的APK包。** 上传完成后即可使用我们提供的测试页进行在线测试(Apk包管理页面的 在线测试)，待测试完成后，再完善下载配置以及web端的集成。
