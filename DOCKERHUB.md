# magpie(非官方镜像)

[magpie](https://github.com/yetone/magpie) 的 Docker 镜像自动同步仓库,内容与官方发布逐字节一致。

- **作者 / 上游仓库**:https://github.com/yetone/magpie
- **官方镜像**:`ghcr.io/yetone/magpie`(官方文档:[usemagpie.ai/docs/docker](https://usemagpie.ai/docs/docker))
- **本同步仓库**:https://github.com/zerone-for/magpie-mirror

每日经 GitHub Actions 用 skopeo 从官方源注册表对注册表复制,digest 由 registry 校验,未经任何改动。

## 标签

| Tag | 说明 |
| --- | --- |
| `latest` | 最新 release |
| `stable` | Stable 渠道对应镜像 |
| `0.1.xxxx` | 版本号标签,保留最近 10 个,可用于固定版本/回滚 |

问题反馈请到上游仓库;本仓库仅为搬运,不含任何修改。
