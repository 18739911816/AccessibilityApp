## 无障碍实现静默安装和静默卸载
实现无障碍功能首先要继承AccessibilityService类，有三个方法可用，onServiceConnected()(当系统成功启动服务时调用，可用于对服务进行配置或弹出提示信息)、onAccessibilityEvent(AccessibilityEvent event)(当指定事件出发该服务时调用，用于实现事件处理的业务逻辑模块)、onInterrupt()(终止accessibility service时调用)。
功能实现主要用到AccessibilityNodeInfo、AccessibilityEvent。AccessibilityEvent封装了所有用户触发的事件，AccessibilityNodeInfo则封装了当前状态视图的属性，即当前状态视图组件以树的形式构建。

在AndroidManifest.xml文件中声明AccessibilityService时，有如下几点必须注意：
+ android:name属性为自定义MyAccessibilityService的绝对类名；
+ 声明"android.permission.BIND_ACCESSIBILITY_SERVICE"权限;
+ 使用<meta-data../>设定AccessibilityService的配置信息，其中，android:name属性的值必须为"android.accessibilityservice"、android:resource属性的值为配置信息，保存在res/xml目录下的accessibility_settings.xml文件，代码如下：
```java
<?xml version="1.0" encoding="utf-8"?>
<accessibility-service
	 android:packageNames="com.android.packageinstaller"
     android:description="@string/accessibility_service_description"
     android:accessibilityEventTypes="typeAllMask"
     android:accessibilityFeedbackType="feedbackGeneric"
     android:accessibilityFlags="flagDefault"
     android:notificationTimeout="100"
     android:canRetrieveWindowContent="true"
  xmlns:android="http://schemas.android.com/apk/res/android" />
```

其中，android:packageNames指明AccessibilityService监听哪个应用程序下的窗口活动，
这里写com.android.packageinstaller表示监听Android系统的安装界面。
android:description指定在无障碍服务当中显示给用户看的说明信息。
android:accessibilityEventTypes指定我们在监听窗口中可以模拟哪些事件，
这里写typeAllMask表示所有的事件都能模拟。
