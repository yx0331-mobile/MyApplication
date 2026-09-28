# 实验1　Android 开发基础　实验报告

| 项目   | 内容                                                 |
| ---- | -------------------------------------------------- |
| 课程名称 | 移动软件开发                                             |
| 实验名称 | 实验1　Android 开发基础                                   |
| 姓名   | 叶翔                                                 |
| 学号   | 121052024018                                       |
| 实验日期 | 2026 年 9 月 22 日                                    |
| 开发工具 | Android Studio Quail 3（AI-261.26222.65 / 2026.1.3） |
| 项目仓库 | <https://github.com/yx0331-mobile/MyApplication>   |

---

## 一、实验目的

1. 掌握 Android Studio 的下载与安装方法；
2. 掌握第一个 Android 工程（Empty Views Activity）的创建流程；
3. 掌握 Git 的基本使用，能够将本地工程推送到 GitHub 远程仓库；
4. 理解 Android 工程忽略文件（`.gitignore`）的作用。

## 二、实验环境

| 环境项                    | 版本 / 参数                                    |
| ---------------------- | ------------------------------------------ |
| 操作系统                   | Windows 11（64 位）                           |
| Android Studio         | Quail 3（Build AI-261.26222.65，版本 2026.1.3） |
| Android Gradle Plugin  | 9.3.3                                      |
| Gradle（Wrapper）        | 9.5.0                                      |
| JDK（Gradle 运行）         | JDK 21                                     |
| compileSdk / targetSdk | API 37（Android 17）                         |
| minSdk                 | API 25（Android 7.1）                        |
| Java 编译级别              | Java 11                                    |
| Android SDK 路径         | `D:\Eason\Android_data`                    |
| Git                    | 2.55.0.windows.3                           |
| 版本控制平台                 | GitHub（账号 `yx0331-mobile`）                 |

主要依赖：`androidx.appcompat:1.6.1`、`androidx.activity:activity-ktx:1.8.0`、`androidx.constraintlayout:2.1.4`、`com.google.android.material:1.10.0`、`junit:4.13.2`。

## 三、实验步骤

### 3.1 预备知识阅读

按实验要求阅读了以下两份资料：

- 课程 CSDN 官方博客（用户名 `fjnu_se`）中关于 Android Studio 安装、参考资料与常见 Bug 解决的说明：  
  <http://blog.csdn.net/fjnu_se/article/details/53734874>
- Android Developer 官方文档：  
  <https://developer.android.google.cn/develop/index.html>  
  <https://developer.android.google.cn/guide/components/fundamentals.html>

通过阅读明确了 Android 的基本组件（Activity、Service、BroadcastReceiver、ContentProvider）以及应用的工程目录结构。

### 3.2 安装 Android Studio

从官网 <https://developer.android.com/studio> 获取安装包并完成安装，安装路径为：

```
C:\Program Files\Android\Android Studio
```

安装完成后指定 Android SDK 存放于 `D:\Eason\Android_data`。首次打开 Android Studio 时可选择 Android SDK 的存放位置，安装过程中会自动配置所需的 SDK 组件。

### 3.3 注册 GitHub 账号并安装 Git

- 在 GitHub 官网注册账号，用户名为 `yx0331-mobile`；
- 从 <https://git-scm.com/> 下载安装 Git for Windows，安装后在命令行通过 `git --version` 验证安装成功（显示 `git version 2.55.0.windows.3`）；
- 配置本地 Git 身份：

```bash
git config --global user.name "yx0331-mobile"
git config --global user.email "2737760282@qq.com"
```

### 3.4 创建第一个 Android 工程

在 Android Studio 中选择 **New Project → Empty Views Activity**，按下表配置：

| 配置项           | 取值                                     |
| ------------- | -------------------------------------- |
| Name          | MyApplication                          |
| Package name  | com.example.myapplication              |
| Save location | D:\Eason\AndroidProjects\MyApplication |
| Language      | Java                                   |
| Minimum SDK   | API 25（Android 7.1）                    |

点击 Finish 后，Android Studio 自动生成标准工程结构并开始 Gradle 同步，同步完成后显示 `BUILD SUCCESSFUL`。工程主要目录如下：

```
MyApplication/
├── app/
│   ├── build.gradle.kts            # 模块级构建脚本（依赖、SDK 版本）
│   ├── proguard-rules.pro
│   └── src/
│       ├── main/
│       │   ├── AndroidManifest.xml # 应用清单文件
│       │   ├── java/com/example/myapplication/MainActivity.java
│       │   └── res/
│       │       ├── layout/activity_main.xml
│       │       ├── values/strings.xml、colors.xml、themes.xml
│       │       └── mipmap/            # 应用图标
│       └── androidTest/、test/        # 测试代码
├── gradle/
│   ├── libs.versions.toml          # 依赖版本统一管理
│   └── wrapper/gradle-wrapper.properties
├── settings.gradle.kts             # 仓库与模块配置
└── local.properties                # 本机 SDK 路径（不提交）
```

**主界面布局文件** `res/layout/activity_main.xml`（采用 ConstraintLayout，居中显示一个 TextView）：

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

**主活动** `MainActivity.java`：

```java
package com.example.myapplication;

import android.os.Bundle;
import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        // 适配系统状态栏 / 导航栏内边距
    }
}
```

> 说明：`MainActivity` 继承 `AppCompatActivity`，在 `onCreate()` 中通过 `setContentView()` 加载布局文件，这是 Android 界面显示的标准写法。

### 3.5 Android 工程的忽略文件

参照 <https://github.com/github/gitignore/blob/master/Android.gitignore> ，Android Studio 新建工程时已自动生成 `.gitignore`，其中忽略了编译产物与本机配置文件：

```
*.iml
.gradle
/local.properties
/.idea/caches
/.idea/libraries
/.idea/modules.xml
/.idea/workspace.xml
.DS_Store
/build
/captures
.externalNativeBuild
.cxx
local.properties
```

验证方式：执行 `git add .` 后通过 `git status` 查看，可见 `build/`、`local.properties`、`.gradle/` 等目录均未被纳入暂存区。

### 3.6 同步工程到 GitHub

首先在 GitHub 网页端新建空仓库 `MyApplication`（**不勾选** README / .gitignore / license，保持空仓库），然后在 Android Studio 内置终端中执行以下命令（实验文档的命令行方式，参考 <http://blog.csdn.net/fjnu_se/article/details/66472625> ）：

```bash
cd D:\Eason\AndroidProjects\MyApplication
git init
git add .
git commit -m "first android project"
git branch -M main
git remote add origin https://github.com/yx0331-mobile/MyApplication.git
git push -u origin main
```

执行 `git push` 时系统自动弹出 GitHub 浏览器授权窗口，点击授权后推送成功。刷新仓库页面可见 `app/`、`gradle/`、`settings.gradle.kts` 等文件，语言统计显示 Java 100%，提交记录为 `first android project`，说明工程已完整上传。

> 补充：另一种方式为 Android Studio 图形界面操作（`VCS → Share Project on GitHub`，参考 <http://blog.csdn.net/fjnu_se/article/details/56683934> ），本实验采用命令行方式完成。

## 四、运行结果

在 Android Studio 中打开 `res/layout/activity_main.xml`，切换到 **Split / Design** 视图，布局编辑器直接渲染出 App 界面，界面中央显示 “Hello World!” 文本。

需要说明的一点：本机未下载 Android 模拟器系统镜像（Device Manager 中 Add Device 列表为空），因此本次实验未使用模拟器运行 App，而是采用 Android Studio 的 **布局编辑器预览（Layout Editor）** 来查看界面效果。布局预览不依赖模拟器，同样能够直观反映 App 的最终呈现。

后续如需运行 App，可：

1. 通过 **Device Manager** 下载系统镜像后创建 AVD；或
2. 开启手机（开发者选项 → USB 调试）后通过数据线真机运行。

## 五、遇到的问题与解决方法

| 序号 | 问题现象                                                                     | 原因                                                                  | 解决办法                                                                           |
| -- | ------------------------------------------------------------------------ | ------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| 1  | Gradle 同步失败：`Could not install Gradle distribution ... Connection reset` | 本机代理拦截了 `services.gradle.org` 的下载请求                                 | 改用国内镜像（腾讯云）下载 Gradle 9.5.0 分发包，校验 SHA256 一致后放入 `~/.gradle/wrapper/dists/` 缓存目录 |
| 2  | 提示 `Project JDK is not defined`                                          | 新建工程未指定 Gradle 运行所用的 JDK                                            | 在工程配置中指定 Gradle JDK 为已安装的 JDK 21                                               |
| 3  | 依赖库（AGP、AndroidX）下载缓慢或失败                                                 | `repo.maven.apache.org` 等官方源在本机网络下不稳定                               | 在 `settings.gradle.kts` 中将阿里云 / 腾讯云 Maven 镜像排在官方源之前，官方源作为兜底                    |
| 4  | 同步卡在下载 `gradle-9.5.0-src.zip`（源码包，约 75 MB）                               | 官方源下载中断后连接挂起，进度停止增长                                                 | 同样由镜像站获取源码包并放入 Gradle 缓存目录，跳过联网下载                                              |
| 5  | 代理相关：Gradle 使用了系统代理导致官方源请求被重置                                            | `~/.gradle/gradle.properties` 中存在 `systemProp.http/https.proxy*` 配置 | 移除全局 Gradle 代理配置，改为直连国内镜像                                                      |

## 六、实验小结

本次实验完成了 Android 开发环境的搭建，从零创建了第一个 Android 工程并将其上传到 GitHub 远程仓库。通过实际操作掌握了：

1. Android Studio 的安装与 SDK 配置；
2. 使用 Empty Views Activity 模板创建 Java 工程的完整流程；
3. Android 工程的标准目录结构与核心文件（`AndroidManifest.xml`、`MainActivity.java`、`activity_main.xml`）的作用；
4. `.gitignore` 对隔离编译产物与本机配置的意义；
5. Git 初始化、提交、关联远程仓库、推送的完整命令流程，以及 GitHub 的浏览器授权推送方式。

实验过程中遇到的主要困难集中在**依赖下载**环节：受本机网络环境影响，Gradle 分发包与依赖库从官方源下载时被拦截导致同步失败。解决思路是**将官方源替换为国内镜像源**，并对 Gradle 分发包采用「镜像下载 + 放入本地缓存」的方式跳过联网环节。这一过程也加深了对 Gradle 构建系统与依赖解析机制的理解。

---

**附：本实验使用的 Git 命令清单**

```bash
git config --global user.name "yx0331-mobile"      # 配置提交用户名
git config --global user.email "2737760282@qq.com" # 配置提交邮箱
git init                                            # 初始化本地仓库
git status                                          # 查看工作区状态
git add .                                           # 暂存所有文件
git commit -m "first android project"               # 提交到本地仓库
git branch -M main                                  # 重命名当前分支为 main
git remote add origin <仓库地址>                     # 关联远程仓库
git push -u origin main                             # 推送并建立上游分支
git log --oneline                                   # 查看提交历史
```
