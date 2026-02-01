# 倒计时器APK构建指南

## 方案一：Web App安装（最简单，推荐）

### 步骤：
1. **打开文件**：直接用手机浏览器打开 `pwa-example.html` 文件
2. **安装到桌面**：浏览器会提示"安装到桌面"，点击安装
3. **像原生应用一样使用**：安装后会在桌面生成图标，点击即可像原生应用一样使用

### 优点：
- ✅ 无需任何技术门槛
- ✅ 支持离线使用
- ✅ 自动更新
- ✅ 占用空间小
- ✅ 支持推送通知

---

## 方案二：在线APK构建工具

### 推荐工具：

#### 1. **AppsConvert** (appsconvert.com)
- 访问网站
- 上传 `example.html` 文件
- 设置应用名称："25分钟倒计时器"
- 设置包名：`com.yourname.countdown`
- 点击生成APK
- 下载安装包

#### 2. **WebViewGold** (webviewgold.com)
- 专业级HTML转APK工具
- 支持自定义图标、启动画面
- 有免费试用版本

#### 3. **APK Editor Studio** (免费)
- 下载APK Editor Studio
- 选择一个简单的WebView APK模板
- 替换其中的HTML文件
- 重新打包

---

## 方案三：本地构建（需要Android Studio）

### 环境准备：
1. **安装Android Studio**
2. **安装Java JDK 8+**
3. **配置Android SDK**

### 详细步骤：

#### 1. 创建新项目
```bash
# 在Android Studio中创建新项目
# 选择 "Empty Activity"
# 项目名称：CountdownTimer
# 包名：com.yourname.countdown
```

#### 2. 修改布局文件 (activity_main.xml)
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</RelativeLayout>
```

#### 3. 修改MainActivity.java
```java
package com.yourname.countdown;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;

public class MainActivity extends AppCompatActivity {

    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        webView = findViewById(R.id.webview);
        
        // 配置WebView
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);
        webSettings.setDomStorageEnabled(true);
        webSettings.setLoadWithOverviewMode(true);
        webSettings.setUseWideViewPort(true);
        webSettings.setSupportZoom(false);
        webSettings.setBuiltInZoomControls(false);
        webSettings.setDisplayZoomControls(false);

        // 加载本地HTML文件
        webView.loadUrl("file:///android_asset/example.html");

        // 设置WebViewClient
        webView.setWebViewClient(new WebViewClient() {
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                view.loadUrl(url);
                return true;
            }
        });
    }

    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            super.onBackPressed();
        }
    }
}
```

#### 4. 添加HTML文件
1. 在 `app/src/main/` 目录下创建 `assets` 文件夹
2. 将 `example.html` 复制到 `assets` 文件夹中

#### 5. 修改AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.yourname.countdown">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="25分钟倒计时器"
        android:theme="@style/AppTheme">
        
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:screenOrientation="portrait">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        
    </application>

</manifest>
```

#### 6. 生成APK
1. 点击 "Build" → "Build Bundle(s) / APK(s)" → "Build APK(s)"
2. 等待构建完成
3. 在 `app/build/outputs/apk/debug/` 目录下找到生成的APK文件

---

## 方案四：使用Cordova/PhoneGap

### 安装Cordova
```bash
npm install -g cordova
```

### 创建项目
```bash
cordova create countdown com.yourname.countdown "25分钟倒计时器"
cd countdown
cordova platform add android
```

### 配置
1. 将 `example.html` 复制到 `www` 目录
2. 修改 `config.xml`：
```xml
<?xml version='1.0' encoding='utf-8'?>
<widget id="com.yourname.countdown" version="1.0.0">
    <name>25分钟倒计时器</name>
    <description>简洁的25分钟倒计时应用</description>
    <author email="your@email.com">Your Name</author>
    <content src="example.html" />
    
    <access origin="*" />
    <allow-intent href="http://*/*" />
    <allow-intent href="https://*/*" />
    
    <platform name="android">
        <allow-intent href="market:*" />
        <icon density="ldpi" src="www/img/icon.png" />
        <icon density="mdpi" src="www/img/icon.png" />
        <icon density="hdpi" src="www/img/icon.png" />
        <icon density="xhdpi" src="www/img/icon.png" />
        <icon density="xxhdpi" src="www/img/icon.png" />
        <icon density="xxxhdpi" src="www/img/icon.png" />
    </platform>
</widget>
```

### 构建APK
```bash
cordova build android
```

---

## 推荐方案

**对于普通用户**：推荐使用 **方案一（Web App安装）**
- 最简单，无需任何技术门槛
- 效果与原生应用几乎相同
- 支持离线使用

**对于开发者**：推荐使用 **方案三（本地构建）**
- 完全可控
- 可以添加原生功能
- 可以发布到应用商店

**快速体验**：推荐使用 **方案二（在线工具）**
- 适合快速测试
- 无需安装开发环境
- 几分钟就能得到APK

---

## 注意事项

1. **权限设置**：确保应用有网络权限（如果需要）
2. **屏幕方向**：建议锁定为竖屏方向
3. **图标设计**：准备不同尺寸的应用图标
4. **测试**：在多个设备上测试APK的兼容性
5. **签名**：发布到应用商店需要签名APK

---

## 故障排除

### Web App无法安装
- 确保使用Chrome或Edge浏览器
- 检查manifest.json文件是否正确
- 确保文件通过HTTPS访问（本地文件可能不支持）

### APK无法安装
- 检查"未知来源"是否已开启
- 确保APK文件完整
- 尝试重新下载APK文件

### 声音不工作
- 检查设备音量设置
- 确保没有开启"勿扰模式"
- 在设置中允许应用通知声音