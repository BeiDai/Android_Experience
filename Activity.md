## Android 界面开发经验

### 1、隐藏标题栏

    在style.xml中修改：

    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">

### 2、背景图片配置

    图片全屏显示：

    android:adjustViewBounds="true"   // 保持图片比例
    android:background="@mipmap/welcomemdpi"  // 设置图片背景
    android:contentDescription="@string/welcome" // 图片注释防止警告
    android:scaleType="fitXY"  // 拉伸图片

    <string name="welcome">欢迎界面</string>

### 3、系统主题配置

**1）系统通常预定义的主题样式**

    Theme.AppCompat               深色主题

    Theme.AppCompat.NoActionBar      没有ActionBar

    Theme.AppCompat.Dialog          对话框适用

    Theme.AppCompat.Dialog.Alert      警告框适用（根据屏幕决定宽度）

    Theme.AppCompat.Dialog.MinWidth    对话框适用（根据内容决定宽度）

    Theme.AppCompat.DialogWhenLarge    充满屏幕（继承自Theme.AppCompat，但没有扩展）

    Theme.AppCompat.CompactMenu      看名字是用于Menu菜单。未验证

    其他主题系统默认都会有上述几种类型的子主题，以此类推就好。

    例如：浅色主题只需要将Theme.AppCompat 替换成 Theme.AppCompat.Light即可

**2）常见的样式属性摘记**

    android:windowFullscreen     隐藏状态栏

    windowActionBar         是否显示ActionBar

    windowNoTitle          是否显示TitleBar，经常和windowActionBar一起使用


### 4、界面跳转

    使用Handler PostDelay

    new Handler().postDelayed(new Runnable(){   
    public void run() {   
        //execute the task   
        }   
     }, delaytime);

### 5、
