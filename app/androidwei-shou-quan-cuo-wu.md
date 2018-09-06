运行Flutter时出错；报错如下：

```
A problem occurred evaluating root project 'android'.
> A problem occurred configuring project ':app'.
   > Failed to install the following Android SDK packages as some licences have not been accepted.
        platforms;android-27 Android SDK Platform 27
     To build this project, accept the SDK license agreements and install the missing components using the Android Studio SDK Manager.
     Alternatively, to transfer the license agreements from one workstation to another, see http://d.android.com/r/studio-ui/export-licenses.html
     
     Using Android SDK: D:\SDK\android-sdk-windows
```

说是许可证未被接收，运行失败，试了下reactNative 原生都可以运行，乜办法还是要解决，方法如下

```
# Build Tools 23.0.1, 24.0.1, 25.0.1
android update sdk --no-ui --all --filter build-tools-25.0.1,android-25,extra-android-m2repository
android update sdk --no-ui --all --filter build-tools-24.0.1,android-24,extra-android-m2repository
android update sdk --no-ui --all --filter build-tools-23.0.1,android-23,extra-android-m2repository
# Alter the versions as required                      ↑               ↑

# -u --no-ui  : Updates from command-line (does not display the GUI)
# -a --all    : Includes all packages (such as obsolete and non-dependent ones.)
# -t --filter : A filter that limits the update to the specified types of
#               packages in the form of a comma-separated list of
#               [platform, system-image, tool, platform-tool, doc, sample,
#               source]. This also accepts the identifiers returned by
#               'list sdk --extended'.

# List version and description of other available SDKs and tools
android list sdk --extended
sdkmanager --list
```

选择自己的版本更新下sdk就OK了，我的版本更新如下：

```
android update sdk --no-ui --all --filter build-tools-27.0.1,android-27,extra-android-m2repository

```



