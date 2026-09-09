# 流量预警助手 (iOS)

单 HTML 页面封装成 iOS 原生 app，用 WKWebView 渲染。

## 项目结构

```
traffic-alert-ios/
├── .github/workflows/build.yml   # GitHub Actions 自动构建
├── ios-app/
│   ├── project.yml                # xcodegen 项目定义
│   ├── Sources/
│   │   ├── App.swift              # 应用入口
│   │   └── ContentView.swift      # UIKit WebView 包装成 SwiftUI
│   └── Resources/
│       └── traffic-alert.html     # 应用本体 HTML（会被复制到 app 包内）
└── README.md
```

## 使用方法

1. **下载整个项目** → 解压到本地
2. **复制 HTML** → 把你最新的 `traffic-alert.html` 覆盖 `ios-app/Resources/traffic-alert.html`
3. **推送到 GitHub** → 新建仓库，代码推上去

   在项目根目录执行：
   ```bash
   git init
   git add .
   git commit -m "init ios app"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/<仓库名>.git
   git push -u origin main
   ```

4. **授权 GitHub Actions** → 仓库 Settings → Actions → General，把 Workflow permissions 改成 "Read and write permissions"，勾选 "Allow GitHub Actions to create and approve pull requests"
5. **Actions 自动触发** → 推完代码后 Actions 自动开始构建，在仓库的 Actions 标签页可看到进度（约 3–5 分钟）
6. **下载 .ipa** → Actions 跑完的绿色记录最下方会有 Artifacts："TrafficAlert.ipa"，点击即可下载

## 安装到手机

由于是 ad-hoc 签名（无证书），安装需要借助第三方工具：

**方法一：Sideloadly** (Windows/Mac，推荐)
- 下载地址：https://sideloadly.io
- 双击打开 Sideloadly，用数据线连上 iPhone
- 把下载的 .ipa 拖进去，输入你的 Apple ID，点 Start
- 手机上会多出一个 app，进入 设置 → 通用 → VPN 与设备管理 → 信任该描述文件即可打开

**方法二：AltStore** (需要 Mac/PC 常驻)
- 先安装 AltServer 到电脑，再用 AltStore app 安装 .ipa

**注意**：免费 Apple ID 签名的 app 每 7 天会过期，需要重新用 Sideloadly 安装一次。付费 Apple 开发者账号 ($99/年) 则签名有效期为 1 年，还能上 TestFlight。

## 数据持久化

app 内的 localStorage 存储在 app 沙盒里，不受 Safari 缓存清理影响。只要你不卸载 app，数据就在。如果要备份数据，用 app 内建的数据管理 → 导出 JSON 功能保存到文件（导出后会用分享面板弹出，选"存储到文件"即可存到 iCloud Drive 或本地 Files）。

## 更新 app

1. 替换 `ios-app/Resources/traffic-alert.html` 为你的新版 HTML
2. commit + push
3. 等 Actions 构建完成，下载安装新的 .ipa（覆盖安装，数据不会丢）
