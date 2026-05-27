# 骑手换电 · 运营商/代理商/租赁公司后台原型

除**品牌方**外，一级/二级代理、合作运营商均可**持有自有设备**（亦可租赁），仅管理自有设备与自有订单；**下级分润**、**合作伙伴分润**为独立菜单。支持**员工 / 合作伙伴登录**与权限控制。支付**架构 B**，平台不抽运营分成。

## 文档

| 文档 | 说明 |
|------|------|
| [docs/PRD.md](docs/PRD.md) | 产品需求主文档 |
| [docs/功能结构与业务流程.md](docs/功能结构与业务流程.md) | 功能结构图、业务流程图（Mermaid） |
| [docs/合作模式与分账.md](docs/合作模式与分账.md) | 分账原则、架构 B、包月确认收入 |
| [docs/代理层级与分润结算.md](docs/代理层级与分润结算.md) | 一二级代理抽成与周期打款 |
| [docs/user-stories.md](docs/user-stories.md) | 用户故事 |
| [docs/acceptance-criteria.md](docs/acceptance-criteria.md) | 原型验收标准 |

## 演示

- **页面**：`prototype/index.html`
- **端口**：`8766`（`python3 main.py`）
- **登录身份**（侧栏下拉）：经营主体 / **员工登录**（如李小运维）/ **合作伙伴登录**（如浦东驿站物业合作方）

## 快速开始

```bash
cd 原型/外卖
python3 main.py
```

访问：http://127.0.0.1:8766/index.html

## 在线预览（团队直接点链接）

通过 **GitHub Pages** 发布 `docs/` 目录（含与 `prototype/index.html` 同步的入口页）：

| 内容 | 链接 |
|------|------|
| **交互原型** | https://lihuoxiu555.github.io/rider-swap-operator-prototype/ |
| **PRD 等文档** | https://lihuoxiu555.github.io/rider-swap-operator-prototype/PRD.html 等（同目录下 `.md` 为纯文本；推荐在 GitHub 仓库 `docs/` 中查看 Mermaid） |

仓库 **Settings → Pages**：Source 选 **Deploy from a branch**，Branch `main`，Folder **`/docs`**。`git push` 后约 1～2 分钟生效。

**改原型后请同步再推送**（Pages 读的是 `docs/index.html`）：

```bash
cp prototype/index.html docs/index.html
git add docs/index.html && git commit -m "sync prototype to pages" && git push
```

其他可选方式：公司内网静态站（Nginx/OSS）、Netlify / Vercel、临时 ngrok（见下文 §4）。

## 团队分享与同步更新（推荐）

本原型 = `prototype/index.html` + `docs/` + `main.py`，**改文件即生效**，无需编译。要让大家看到同一份、且你更新后他们能跟上，建议用 **Git 仓库**（GitHub / GitLab / Gitee / 公司 Git）。

### 1. 你：首次推到远程（一次性）

在 `原型/外卖` 目录（或把该目录单独建仓）：

```bash
cd 原型/外卖
git init
git add prototype docs README.md main.py requirements.txt
git commit -m "骑手换电后台原型与文档"
# 在 GitHub/Gitee 新建空仓库后：
git remote add origin <你的仓库 HTTPS 或 SSH 地址>
git branch -M main
git push -u origin main
```

不要把含密钥的 `.env` 等文件放进仓库；本目录一般只有 HTML/MD，可直接提交。

### 2. 同事：拉取与本地预览

```bash
git clone <仓库地址>
cd 外卖   # 进入克隆下来的目录名
python3 main.py
```

浏览器打开终端里打印的地址（默认 http://127.0.0.1:8766/index.html）。

### 3. 你更新后：同事如何同步

你本地改完并提交推送：

```bash
git add -A
git commit -m "说明本次改了什么"
git push
```

同事在已有克隆目录执行：

```bash
git pull
```

然后**刷新浏览器**（必要时强刷 `Cmd+Shift+R`）即可看到最新原型。文档（`docs/*.md`）同样随 `git pull` 更新。

### 4. 仅「给别人看一眼」、不要求 Git 时

| 方式 | 适用 | 说明 |
|------|------|------|
| 压缩包 | 评审/外包 | 打包 `prototype` + `docs` + `main.py` 发邮件/网盘；**无法自动同步**，需你重新发 zip |
| 内网静态站 | 产品/设计常驻预览 | 把 `prototype/` 挂到公司 Nginx/OSS；你更新后重新上传或 CI 自动部署 |
| 临时外网 | 远程演示 | 本机 `python3 main.py` 后可用 ngrok 等暴露端口（注意安全，仅演示） |

### 5. 协作约定（避免互相覆盖）

- **产品/设计**：主要改 `docs/`，原型交互改 `prototype/index.html`
- 提交前 `git pull`，减少冲突；冲突多在 `index.html`，用编辑器合并即可
- 大改前可 `git checkout -b feature/xxx` 提 Merge Request，方便评审

### 6. 文档里的流程图

`docs/功能结构与业务流程.md` 等为 Mermaid，在 **GitHub / GitLab / VS Code / Cursor** 打开 md 即可渲染；与 HTML 原型分开预览即可。
