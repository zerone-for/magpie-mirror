# magpie-mirror

把 [yetone/magpie](https://github.com/yetone/magpie) 的 Docker 镜像从 `ghcr.io/yetone/magpie`
自动同步到 Docker Hub(`docker.io/<你的用户名>/magpie`),绕开 ghcr.io 访问不稳定的问题。

官方只发布到 ghcr.io(见其 docker*.yml workflows),本仓库只是一个**非官方搬运镜像**,
skopeo 直接做注册表到注册表的复制,内容哈希由 registry 校验,不经手改动。

## 同步范围

- `latest` — 最新 release
- `stable` — 桌面版 Stable 渠道对应镜像

不同步:`daily`(与 latest 高度重合)、`edge`(随 main 每次提交变动,太频繁)、历史版本号 tag。

## 一次性的设置

1. 在 Docker Hub 创建 **Access Token**:Account Settings → Security → New Access Token
   (权限 Read/Write 即可,不要用账号密码)。
2. 在 GitHub 创建公开仓库,推入本目录内容:
   ```sh
   cd magpie-mirror
   git init && git add -A && git commit -m "magpie mirror sync"
   git remote add origin git@github.com:<你的用户名>/magpie-mirror.git
   git push -u origin main
   ```
3. 仓库 Settings → Secrets and variables → Actions,添加两个 secret:
   - `DOCKERHUB_USERNAME` — Docker Hub 用户名
   - `DOCKERHUB_TOKEN` — 上一步的 Access Token
4. 到 Actions 页手动 Run workflow 一次,验证首同步成功,之后每天 UTC 05:17 自动跑。

## 日常使用

NAS 上拉取:

```sh
docker pull <你的DockerHub用户名>/magpie:latest
```

digest 与 ghcr 官方一致(可用 `skopeo inspect` 对比核验)。

## 注意

- GitHub 会停用 60 天无活动的定时 workflow;workflow 内置了每月 1 号的空 commit 保活,
  平时不需要管。
- 若 Docker Hub 仓库被删或 token 失效,Actions 会在日志里报 401/403,重新生成 token 更新 secret 即可。
