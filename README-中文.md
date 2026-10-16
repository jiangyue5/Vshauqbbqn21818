# NOIR OS — GitHub APK 构建包

这个压缩包把上传的 `index_1.html` 放进 Capacitor 项目，并附带 GitHub Actions 自动构建流程。
它会构建一个 **Android Debug APK**，适合先安装测试；不是已签名的正式发布包。

## 用 GitHub 网页构建 APK（手机也可以）

1. 解压这个 ZIP。
2. 打开 GitHub 并新建一个仓库，例如 `noir-os-android`。
3. 进入仓库，选择 **Add file → Upload files**，上传解压后文件夹里的所有内容。
   - 要上传的是 `package.json`、`capacitor.config.ts`、`www` 文件夹和 `.github` 文件夹。
   - 不要把外层 `noir-github-apk` 文件夹整个再套一层上传；`.github/workflows/android-apk.yml` 必须在仓库根目录下。
4. 点击 **Commit changes**。
5. 打开仓库的 **Actions** 标签页，进入 **Build Android APK** 工作流。
6. 如果没有自动运行，点 **Run workflow → Run workflow**。
7. 等工作流变成绿色成功状态，点进这次运行，在页面下方 **Artifacts** 区域下载 `NOIR-OS-debug-APK`。
8. 下载后解压 Artifact ZIP，里面的 `app-debug.apk` 就是安装包。把 APK 下载到安卓手机后，允许浏览器/文件管理器安装应用即可。

## 后续更新 HTML

1. 在仓库中打开 `www/index.html`。
2. 点击铅笔编辑，或删除后重新上传更新后的 HTML。
3. Commit changes。
4. 到 **Actions** 查看新一轮构建，成功后下载新的 Artifact。

## 重要说明

- 这是把现有 HTML 装进 Android WebView 的包装项目，不会自动把网页改写成原生 Android 功能。
- HTML 里使用的外部字体/网络资源需要设备联网才能加载。
- GitHub Actions 生成的是测试用 Debug APK。若要公开发布，需进一步配置正式签名。
- 这个流程每次构建时会自动生成 `android` 原生工程，所以压缩包里没有预先生成的 `android` 目录。
- 如果 GitHub Actions 报错，请打开失败步骤，把红色错误内容复制出来，我可以按报错继续修复。


## 本次通知增强

已加入原生 Android 通知支持：

- Android 13+ 首次使用通知功能时会申请 `POST_NOTIFICATIONS` 权限。
- 角色收到新消息时，会通过原生 Android Notification Channel 推送系统通知。
- 通知使用高优先级消息通道，支持后台状态下的通知提示。
- 通知设置为 `VISIBILITY_PUBLIC`，因此在系统允许的情况下可显示在锁屏通知区域。
- 点击通知可以重新打开 NOIR OS。
- 保留浏览器 Notification 作为非原生环境的后备方案。
- GitHub Actions 会在 `npx cap add android` 后自动注入原生通知插件，因此不需要把生成的 `android` 目录提交进仓库。

### 注意

这是“后台/锁屏系统通知”，不是强制全屏打断式通知。Android 系统、省电策略、用户的通知权限以及锁屏隐私设置仍可能影响显示方式。

如果 Android 系统已经把 NOIR OS 的通知权限关闭，需要在系统设置里重新允许通知。
