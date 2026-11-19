# OmniBox 极客 + NewWex 合并验证版（97 源）

本项目合并以下两个订阅：

- 极客：<https://jikefuye.cn/tvbox.json>
- NewWex：<https://9280.kstore.vip/newwex.json>

共读取 198 条原始配置；按 key、API、ext 和实际业务入口核对后得到 101 个候选入口，最终保留 97 个。

## 合并原则

1. 同一业务源只保留一个入口，不能只按展示名称判断。
2. 需要 Cookie、Token、Emby、AList 或用户授权的源不视为无效。
3. **配置中心**和**我的网盘**必须保留，网盘 Cookie/授权仍在 OmniBox 后台配置。
4. 只有有明确失败证据的源才剔除；第三方服务状态变化后可再次复测。
5. 极客独有的“极品听书”继续保留；NewWex 新增且在线验证成功的“全盘综合”已加入。

## 全盘综合配置

无需配置即可使用默认服务。可选环境变量：

- `QUANPAN_API`：默认 `https://quanpan.xyz`
- `QUANPAN_MAX_PER_TYPE`：每种网盘最多返回数量，默认 30
- `QUANPAN_MAX_RESULTS`：总返回上限，默认 180
- `PANCHECK_API`：可选，配置后用于批量过滤失效分享链接

## 荐片播放兼容

“贱片秒播”默认使用原生 Guard 的 Android UA 与 Referer 直连播放，不再发送多余的 Origin。若个别 CDN 在你的网络下仍然较慢，可给该源设置 `JIANPIAN_PLAY_HEADER_MODE=none` 测试无请求头直连；需要自定义 Referer 时使用 `JIANPIAN_REFERER`。

## 播放性能

- 18 个网盘类源默认线路已调整为 `直连 → 本地代理 → 服务端代理`，电视端优先直连 CDN。
- 17 个源中的 19 条播放增强链路，刮削/弹幕等待限制为最多 500ms，避免已经获得播放地址后仍长时间转圈。
- 可通过 `PLAY_ENHANCEMENT_TIMEOUT_MS` 调整等待时间；设置为 `0` 表示完全不等待播放增强。
- 仅删除了与原生 Guard 不一致、可确认多余的播放 `Origin`；站点鉴权或防盗链确实需要的请求头仍然保留。

## 上传 GitHub

上传前，将所有脚本中的 `YOUR_GITHUB_USER/OmniBox-Jike-NewWex-Union` 替换为你的 GitHub 用户名和仓库名。不要把网盘 Cookie、密码或 Token 写进公开仓库。

完整核对记录见 [MERGE-REPORT.md](MERGE-REPORT.md)，机器可读清单见 [manifest.json](manifest.json)。
