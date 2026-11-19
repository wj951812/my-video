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

“贱片秒播” 1.4.0 已恢复原生 `WexAiJianPianGuard` 的关键逻辑：启动时通过 DNS TXT 发现 `wangerniu.*` 动态主站，并在播放时返回荐片原生 Android UA 与当前主站 Referer。OK影视因转发请求头而显示 `127.0.0.1:9980/m3u8.m3u8` 属正常现象，该本地代理用于向 CDN 注入必要请求头。仅在明确不需要鉴权时才设置 `JIANPIAN_PLAY_HEADER_MODE=none`；需要自定义 Referer 时使用 `JIANPIAN_REFERER`。

## 播放性能

- 除“虎斑 4K”外，其余 17 个网盘类源已恢复原 Guard 的默认线路 `本地代理 → 服务端代理 → 直连`，使 OK影视能通过本地 HLS 代理注入网盘播放所需的 Cookie/UA/Referer。
- 17 个源中的 19 条播放增强链路，刮削/弹幕等待限制为最多 500ms，避免已经获得播放地址后仍长时间转圈。
- 可通过 `PLAY_ENHANCEMENT_TIMEOUT_MS` 调整等待时间；设置为 `0` 表示完全不等待播放增强。
- 不再批量删除播放 `Origin`；伯伯、虎牙、魔都、七味、立播已恢复原实现的 Origin/Referer/UA 组合。

## 虎斑 4K 服务端播放模式

- `虎斑-4K.js` 1.2.0 已参照 CatPaw/UZ 订阅的播放链路，默认线路调整为 `服务端代理 → 直连`，不再生成“本地代理”入口。
- 默认域名更新为实测正常的 4 个入口，移除两个已无法连接的旧 IP。
- 电视端链路为 `电视 → OmniBox/NAS → 网盘/CDN`；网盘 Cookie 和授权仍取自 OmniBox 配置中心，脚本中不保存凭据。
- 如果 NAS 带宽或 CPU 不适合全程代理，可设置 `SOURCE_NAMES_CONFIG=直连` 恢复直连模式。

## 上传 GitHub

上传前，将所有脚本中的 `YOUR_GITHUB_USER/OmniBox-Jike-NewWex-Union` 替换为你的 GitHub 用户名和仓库名。不要把网盘 Cookie、密码或 Token 写进公开仓库。

完整核对记录见 [MERGE-REPORT.md](MERGE-REPORT.md)，机器可读清单见 [manifest.json](manifest.json)。
