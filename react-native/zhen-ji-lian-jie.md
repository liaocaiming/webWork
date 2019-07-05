###### 1. 首先确保你的电脑和手机设备在同一个 Wi-Fi 环境下
###### 2.运行adb reverse tcp:8081 tcp:8081
###### 3.在设备上运行你的 React Native 应用。和打开其它 App 一样操作。在设备上运行你的 React Native 应用。和打开其它 App 一样操作
###### 4.adb shell input keyevent 82，可以打开开发者菜单;
###### 5. 点击Dev Settings -> Debug server host for device


# 如果你不是通过Android Studio安装的sdk，则其路径可能不同，请自行确定清楚。
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/emulator