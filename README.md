# Hust课表 · 下载页（GitHub Pages 发布目录）

这个文件夹里的东西就是要传到 GitHub 的内容：

| 文件 | 说明 |
| --- | --- |
| `index.html` | 下载页（单文件，74 KB，二维码按当前网址实时生成，无外部依赖） |
| `hust-timetable.apk` | 安装包（57.5 MB，与页面同目录，页面用相对路径 `hust-timetable.apk` 下载） |
| `.nojekyll` | 空文件，告诉 GitHub Pages 不要用 Jekyll 处理，保证静态文件原样发布 |
| `README.md` | 本说明，传上去也行、删掉也行（不影响页面） |

---

## 一、上传到 GitHub

> ⚠️ **GitHub 网页端拖拽上传单文件上限是 25 MB，而 APK 有 57.5 MB，网页传不上去**，请用 git 命令行或 GitHub Desktop。

### 1. 在 GitHub 上新建一个空仓库
`New repository` → 名字例如 `hust-timetable` → **Public**（Pages 免费版要求公开仓库）→ 不要勾选 README / .gitignore / license（保持空仓库）。

### 2. 在本文件夹里初始化并推送（把 `<你的用户名>` 换成你的 GitHub 用户名）

```powershell
cd C:\Users\59302\Desktop\hust-timetable-pages

git init
git branch -M main
git add -A
git commit -m "Hust课表 下载页 v1.0.0"
git remote add origin https://github.com/<你的用户名>/hust-timetable.git
git push -u origin main
```

第一次 `push` 会弹出 Git 凭据窗口，输入 GitHub 账号 + **Personal Access Token**（GitHub 早就不支持用登录密码推代码了；在 GitHub → Settings → Developer settings → Personal access tokens 生成，勾 `repo` 权限）。

也可以用 **GitHub Desktop**：Add local repository → 选这个文件夹 → Publish repository（勾掉 "Keep this code private"）。

> 本机网络提示：这台电脑访问 `github.com:443` 是被阻断的（`api.github.com` 通）。推送时如果报 `Connection was reset`，先开代理/VPN 再 `git push`。

## 二、开启 Pages

仓库页面 → **Settings → Pages** → Build and deployment：
- **Source**：`Deploy from a branch`
- **Branch**：`main` / 目录选 `/ (root)` → **Save**

等 1~2 分钟，页面顶部会出现站点地址，形如：

```
https://<你的用户名>.github.io/hust-timetable/
```

## 三、链接与二维码

- 这个地址就是分享给同学的**下载页链接**。
- 页面里的二维码是**打开页面时按当前网址生成的**（`location.href`），所以它自动就是你 Pages 的地址，不用重新生成页面；微信 / QQ 里长按二维码可直接识别进入。
- 页面底部「复制本页链接 / 复制下载直链」两个按钮方便转发。

## 四、以后发新版本

在项目里跑一条命令，然后再推一次这两个文件就行：

```powershell
# 在 C:\Users\59302\Desktop\课表APP 下
pwsh -File publish\publish_release.ps1 -Version 1.1.0 -Notes "修复xxx"
# 脚本会重新构建 APK、更新页面上的版本/大小/SHA256、生成 update.json

# 然后把新文件覆盖到这个文件夹并推送
copy C:\Users\59302\Desktop\课表APP\publish\index.html            C:\Users\59302\Desktop\hust-timetable-pages\index.html
copy C:\Users\59302\Desktop\课表APP\publish\hust-timetable.apk    C:\Users\59302\Desktop\hust-timetable-pages\hust-timetable.apk
cd C:\Users\59302\Desktop\hust-timetable-pages
git add -A; git commit -m "v1.1.0"; git push
```

记得同时把项目里 `pubspec.yaml` 的 `version: x.y.z+N` 的 **N 递增**（以后做应用内更新时会用到）。

## 五、几个要注意的地方

1. **单文件 100 MB 上限**：APK 现在 57.5 MB 能推；哪天超过 100 MB 就必须改用 OSS/网盘等外部直链。
2. **仓库会变大**：每发一版 git 历史里都会多留一份约 59 MB 的 APK。发十几个版本后建议新建仓库重来，或者把 APK 挪到对象存储，只让页面留在 Pages（把 `index.template.html` 里的 `CONFIG.apkUrl` 改成 APK 的绝对直链，重跑 `build_page.ps1`）。
3. **国内访问 `github.io` 不稳定**：校园网/移动网络下经常很慢或打不开，同学反馈打不开时，把同样这两个文件放到 Gitee Pages 或阿里云 OSS 再发一份链接即可（页面不用改，二维码会跟着新地址走）。
4. **微信里不能直接下载 APK**：页面已经做了提示条（「点右上角 ⋯ → 在浏览器打开」），分享时最好也补一句提醒。
