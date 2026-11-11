# magpie-mirror

[magpie](https://github.com/yetone/magpie) 的 Docker 镜像自动同步器:每天定时比对官方
`ghcr.io/yetone/magpie` 与 Docker Hub 上的镜像 digest,有更新就用 skopeo 注册表对注册表复制,
内容逐字节一致(digest 由 registry 校验),未经任何改动。

- **magpie 作者**:yetone — https://github.com/yetone/magpie
- **官方镜像**:`ghcr.io/yetone/magpie`(官方文档:[usemagpie.ai/docs/docker](https://usemagpie.ai/docs/docker))
- **同步产物**:https://hub.docker.com/r/zeronefour/magpie

## 使用镜像

```sh
docker pull zeronefour/magpie:latest      # 最新 release
docker pull zeronefour/magpie:stable      # Stable 渠道对应镜像
docker pull zeronefour/magpie:0.1.1150    # 固定版本,可回滚
```

ghcr.io 访问不稳的环境,直接拉这个就行。

## 同步范围

| Tag | 说明 |
| --- | --- |
| `latest` | 最新 release(移动标签) |
| `stable` | Stable 渠道对应镜像(移动标签) |
| `0.1.xxxx` | 版本号标签,保留**最近 10 个**,新版本自动补同步 |

不同步:`daily`(与 latest 高度重合)、`edge`(随 main 每次提交变动,太频繁)、
更早的历史版本(每个版本约几十 MB,全量回填不现实;如需某个老版本,改大
workflow 里的 `VERSION_KEEP` 再手动触发一次即可)。

## 自己搭一个同样的

相同需求(把任意 ghcr 镜像搬到 Docker Hub)可以复用这套流程:

1. Fork 本仓库(或复制 [.github/workflows/sync.yml](.github/workflows/sync.yml) 到你的仓库)
2. 改 workflow 顶部的 `SRC`、`MOVING_TAGS`、`VERSION_KEEP` 为你要同步的官方镜像与 tag 规则
3. 改两个同步步骤里的 `DST` 环境变量,指向 `docker.io/<你的用户名>/<仓库名>`
4. 到仓库 Settings → Secrets and variables → Actions 添加:
   - `DOCKERHUB_USERNAME` — Docker Hub 用户名
   - `DOCKERHUB_TOKEN` — Docker Hub Access Token(Account Settings → Security → New Access Token,**Read, Write & Delete** 权限;网页 API 改描述需要 Delete 位)
5. 推送即首同步,之后每天 UTC 05:17 自动比对更新

注意事项:

- 同步的是公开镜像,无需 ghcr 凭据;目标仓库首次推送时 Docker Hub 会自动创建
- GitHub 会对 60 天无活动的仓库停用定时 workflow,workflow 内置了每月 1 号的空 commit 保活
- 搬运他人镜像请保留来源与作者署名,标注非官方,不要误导为官方渠道
