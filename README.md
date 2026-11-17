# freebie-radar-lzcapp

[资源雷达（Resource Radar）](https://github.com/ZiChuanLan/freebie-radar) 的懒猫微服打包：多兴趣社区信息聚合与精准推送工作台。上游 MIT 开源。

- 打包方式：**源码构建**（上游未发布 docker 镜像）。CI 用上游 Dockerfile 逻辑构建并内嵌 LPK（dockerfile-embed，toolchains: docker）
- 版本策略：跟随上游源码版本（git version source），打 tag `v*` 触发构建发布
- 只发布到喵喵商店（`stores.private`），不走官方商店

## 与上游 docker compose 部署的差异

| 项 | 上游 compose | 本打包 |
|----|-------------|--------|
| 镜像 | 本地 build | CI 构建后内嵌 LPK |
| `ADMIN_TOKEN` | `.env` 手填 | 平台稳定密钥 `{{ stable_secret }}`（重装不变） |
| `ADMIN_HOSTS` | 留空 | `{{.S.AppDomain}}` 等（管理 API 接受懒猫网关 Host） |
| 数据 | `./data` | `/lzcapp/var/data` |
| 端口 | 127.0.0.1:8788 | 平台网关反代 `radar:8788` |

## 自动发布

`.github/workflows/lazycat.yml` 使用 [ca-x/lazycat-github-action](https://github.com/ca-x/lazycat-github-action) `v1`：手动或定时触发，`toolchains: docker` 从内嵌 Dockerfile 构建镜像、打包 LPK、发 GitHub Release（版本化附件）并发布到喵喵商店。Secrets：`APPSTORE_URL`、`APPSTORE_TOKEN`（组织级）。

## 首次使用

1. 打开应用即可浏览公开资源流；管理区密码为部署时生成的 `ADMIN_TOKEN`（SSH 到盒子可从应用环境变量查看，建议进入管理区后修改）。
2. 管理区配置 Telegram Bot、AI 模型（OpenAI 兼容）、屏蔽词、数据源启停与采集周期，全部实时热修改。
