MinIO MC GUI（Windows 便携版）

## 下载

程序二进制不在仓库里（体积原因），请到 **Releases** 下载唯一的文件：

- `MinIO-MC-GUI.exe` —— 单文件便携版，内置官方 mc.exe，**放到可写目录双击即可**

校验值见本仓库 `MinIO-MC-GUI.exe.sha256`：

```powershell
Get-FileHash .\MinIO-MC-GUI.exe -Algorithm SHA256
```

首次启动会自动在同目录创建 `app-data\`（WebView2 数据 + 释放出来的 mc.exe）和 `mc-config\`（连接配置）。

## 使用

双击 Start-MinIO-MC-GUI.cmd 或 MinIO-MC-GUI.exe。
支持 MinIO 以及任意 S3 兼容对象存储（AWS S3、Ceph RGW、各类私有化 S3 服务等）。

使用流程：
1. 点左侧「+ 连接 MinIO」，填写连接名称、服务地址、桶名称（可留空）、Access Key、Secret Key。
2. 保存后会自动连接；左侧「连接」列表里的每个名称就是一个连接，点名称即可切换，可建多个连接轮换使用。
3. 对象列表会递归显示该连接下的全部对象，支持搜索与刷新。
4. 下载：点对象右侧「下载」→ 选择保存位置（另存为）→ 在「传输」页看进度。
5. 上传：把文件或文件夹直接拖进对象列表，或点右上角「选择文件」；上传到该连接的桶根目录。
6. 删除：点对象右侧「删除」并确认。删除不可恢复，请谨慎操作。
7. 分享：点对象右侧「分享」→ 选「1 小时 / 1 天 / 7 天」生成**预签名链接**（到期自动失效，可直接发给客户/同事下载），或点「复制直链」（该链接要求桶/对象可匿名读取）。
8. 「传输」页只显示上传/下载进度；「运行日志」页是 mc 的原始输出，排查问题时看这里。

关于 SmartScreen 提示（重要）：
本程序未购买代码签名证书，Windows 可能弹出「Windows 已保护你的电脑」。
这是对未签名程序的信誉提示，不是病毒告警。处理方法任选：
- 点提示中的「更多信息」，再点「仍要运行」。
- 或右键 exe → 属性 → 勾选「解除锁定」→ 确定。
- 或命令行执行：Unblock-File -Path "MinIO-MC-GUI.exe"
- 分发给别人时：先右键 zip → 属性 → 解除锁定，再解压；解压出来的 exe 通常不再提示。
想确认文件有没有被篡改：
- 同目录的 MinIO-MC-GUI.exe.sha256 记录了官方哈希，
  用 Get-FileHash MinIO-MC-GUI.exe -Algorithm SHA256 对比即可。

程序运行后会在同目录创建：
- app-data：WebView2 运行数据，以及内置 mc.exe 的释放副本（首次启动生成）
- mc-config：mc 的连接配置与凭据（含 Access Key / Secret Key，请保护整个目录，不要外发）

TLS 校验保持开启。便携包已附带当前 S3 服务端所需的 Sectigo 根证书：
mc-config\certs\CAs\Sectigo-Public-Server-Authentication-Root-R46.crt

请把整个文件夹放在有写权限的位置（例如用户目录或非系统盘）。
不要把 mc-config\config.json 发给别人。
