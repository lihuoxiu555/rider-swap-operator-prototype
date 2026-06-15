# 骑手换电 · 运营商 / 渠道商 / 资金方后台原型

运营商可**持有自有设备**（亦可向资金方租赁），管理自有设备与订单；**合作伙伴分润**为独立菜单。支持**员工 / 合作伙伴登录**与权限控制。支付**架构 B**，平台收取 1% 技术服务费（C 端分账 + B 端渠道人天确认消耗）。

## 文档

**原型内浏览（推荐）**：启动 `main.py` 后打开 [prototype/docs/index.html](prototype/docs/index.html)，侧栏可切换全部 Markdown 文档（含 Mermaid 源码、表格）。

| 文档 | 说明 |
|------|------|
| [docs/PRD.md](docs/PRD.md) | 产品需求主文档 |
| [docs/角色与功能清单.md](docs/角色与功能清单.md) | 四角色功能矩阵与风险 |
| [docs/换电场景与运营商结算.md](docs/换电场景与运营商结算.md) | 跨运营商清分、保证金/信用额度 |
| [docs/功能结构与业务流程.md](docs/功能结构与业务流程.md) | 功能结构图、业务流程图（Mermaid） |
| [docs/合作模式与分账.md](docs/合作模式与分账.md) | 分账原则、架构 B、包月确认收入 |
| [docs/天数池.md](docs/天数池.md) | 人天额度池：预占确认/规则/团队/零售价/账本（原型 9 Tab） |
| [docs/骑手端PRD.md](docs/骑手端PRD.md) | 骑手小程序 PRD |
| [docs/user-stories.md](docs/user-stories.md) | 用户故事 |
| [docs/acceptance-criteria.md](docs/acceptance-criteria.md) | 原型验收标准 |

## 演示

| 端 | 路径 | 说明 |
|----|------|------|
| **运营商后台** | `prototype/index.html` | 运营商 / 渠道商 / 资金方 |
| **骑手端（移动）** | `prototype/mobile/index.html` | 参考鲸浪出行布局；三 Tab + 扫码换电 |
| **产品文档** | `prototype/docs/index.html` | 全部 PRD / 业务规则 / 验收标准 |

- **端口**：`8766`（`python3 main.py`）
- **登录身份**（侧栏下拉）：经营主体（运营商 / 渠道商 / 资金方）/ **员工登录** / **合作伙伴登录**
- **人天额度池**：渠道商登录 → 侧栏「人天额度池」；演示 `QP-2601` 低余额预警、预占失败、续费重试

## 快速开始

```bash
cd 原型/外卖
python3 main.py
```

访问：http://127.0.0.1:8766/prototype/index.html（根路径 `/` 自动跳转）

## 在线预览（团队直接点链接）

通过 **GitHub Pages** 发布 `docs/` 目录（含与 `prototype/index.html` 同步的入口页）：

| 内容 | 链接 |
|------|------|
| **运营商后台原型** | https://lihuoxiu555.github.io/rider-swap-operator-prototype/ |
| **产品文档浏览器** | https://lihuoxiu555.github.io/rider-swap-operator-prototype/documentation/ |
| **骑手端原型** | https://lihuoxiu555.github.io/rider-swap-operator-prototype/mobile/ |

侧栏「→ 产品文档 / 骑手端」在本地（`/prototype/`）与 GitHub Pages 会自动跳转到对应路径。

仓库 **Settings → Pages**：Source 选 **Deploy from a branch**，Branch `main`，Folder **`/docs`**。`git push` 后约 1～2 分钟生效。

**改原型后请同步再推送**（Pages 读的是 `docs/index.html`）：

```bash
# 仅复制到 docs/
./scripts/sync-pages.sh

# 同步 + 提交 + 推送（推荐）
./scripts/sync-pages.sh -c -p -m "更新原型：说明本次改动"
```
