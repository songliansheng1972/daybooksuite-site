# daybooksuite.com 上线清单

> 一页看完。每步只需勾选；**放行硬闸**任一未过即**不得上线**。
> 细则与文件对照见 `README.md`；本页是**执行页**。

---

## 〇 · 放行硬闸（一票否决 · 上线前逐条核）

| # | 硬闸 | 怎么判（可核） | 现状 |
|---|---|---|---|
| **H0** | **部署通道**已通（改完能一键上线·非手工重传） | 见 §一b | ⛔ **未通（当前手工上传）** |
| **H1** | Buy 按钮已换**真链** | `grep` 无 `data-placeholder`（见 §四） | ⛔ 未过（占位中） |
| **H2** | Paddle `successUrl` 指向本站付款后页 | Paddle 后台已填（见 §三） | ⛔ 未设 |
| **H3** | Paddle 交付方式 = **Download**（发下载邮件·零后端） | Paddle 后台 | ⛔ 未设 |
| **H4** | 域名邮箱 `hi@daybooksuite.com` 已通 | 发一封、收回执 | ⛔ 待 Email Routing |
| **H5** | 正式域名 HTTPS 可访问 | 浏览器直访 `https://daybooksuite.com` | ⚠️ **已上线·但线上是旧版** |
| **H6** | 三政策页在线可达（Terms / Privacy / Refund） | 逐页打开 | ✅ 线上可达（均 200） |
| **H7** | 付款后落地页 `thank-you.html` 在线 | 直访 `/thank-you.html` | ⛔ **线上缺失**（被回退掩盖·见 §〇b） |

> **H1 与 H2 是同一件事的两面**：Buy 真链点了要能跳 Paddle 结账，付完要能回到本站 `thank-you.html`。**两条都过，才算付款闭环通。**

---

## 〇b · 线上现状诊断（2026-10-12 19:52 亲核）

| 项 | 实测 |
|---|---|
| 托管 | **Cloudflare**（`server: cloudflare`） |
| 首页 `/` | **旧版**：`$9.9` ×2；卡片写 `(账 + 录)` |
| 中文页 `/zh` | **旧版**：卡片写 `账与录（基座）` |
| `/terms`、`/refund` | **旧版**：`US$9.9` |
| `/thank-you.html` | ⛔ **不存在**（返回的是首页） |
| 任意不存在路径（如 `/abc`） | ⚠️ **200 + 首页**（SPA 回退·本该 404） |

**⇒ 病根：改了本地 ≠ 上线。** 上次上传之后，本地所有改动（定价 $10.99、起居注正名、规模段）**都没上站**。
**⇒ 两个必须堵的洞**：①**`thank-you.html` 线上缺失**（Paddle `successUrl` 会落空）；②**SPA 回退**（404 变 200）。

---

## 一 · Cloudflare Pages 部署（推荐 · 与域名同一后台）

1. **自己敲网址**登录 `dash.cloudflare.com`（**不点邮件里的链接**）。
2. 左侧 **Workers & Pages → Create → Pages → Upload assets**。
3. 上传 `~/Desktop/daybooksuite-site/` 下**全部网页文件**（`index.html`／`zh.html`／`terms.html`／`privacy.html`／`refund.html`／`thank-you.html`／`style.css`）。
   - ⚠ **不要上传 `.git` 目录**（本地上传时只选那 7 个文件即可）。
4. 得到 `xxx.pages.dev` 临时地址 —— **先直访验证**：首页、中文页、三政策页、`thank-you.html` 都能打开。
5. **Pages → Custom domains → 绑 `daybooksuite.com`**（自动签发 HTTPS）。

> 备用：GitHub Pages（README §上线·乙）。用二者之一，**均无需备案**。

---

## 一b · 部署通道（根治「改了本地不更新」· 三选一）

> **H0 硬闸**：只要还停在「手工拖拽上传」，每次改完都可能忘传——**这次就是这么漏的**。

- **甲（推荐·一劳永逸）**：站点入 GitHub 仓 → Cloudflare Pages **连该仓** → 以后 `git push` 即自动上线（本地提交历史已在）。
  - ⚠ 若现有 Pages 项目是用 **Upload assets** 建的，**不能**改成 Git 集成，须**新建一个 Pages 项目**（域名重绑）。
- **乙（最快·可复用现有项目）**：装 `wrangler` → `wrangler login`（浏览器授权一次）→
  `wrangler pages deploy ~/Desktop/daybooksuite-site --project-name=<现有项目名>`。
- **丙（不给凭据·您自己来）**：控制台 → 该项目 → **Create new deployment** → 重新拖入本目录 7 个文件。

**无论走哪条，本次都要一并修那两处**：①补 `thank-you.html`；②把 SPA 回退改成 404（或只对已知路径回退）。

---

## 二 · 域名与邮箱（与 §一 并行）

- **DNS**：Cloudflare 会分配两个 NS 地址（形如 `xxx.ns.cloudflare.com`）→ 去**域名注册商**处改成这两个 → 等待生效（通常几分钟到几小时）。
- **域名邮箱**：Cloudflare 后台 **Email Routing** → 添加 `hi@daybooksuite.com` → 转发到你的 sina 邮箱。
- **邮件提醒**：Cloudflare 会发三类信（邮箱验证 / 改 NS / 站点已激活），发件人都是 `@cloudflare.com`。
  - 你贴的那段官网文案**不是**确认信（是首页宣传页）。
  - **一律不点邮件里的链接**，直接登录后台看状态。

---

## 三 · Paddle 侧（决定 Buy 能否放行）

| 项 | 该设成 | 备注 |
|---|---|---|
| 商品价格 | **US$10.99 一次性** | 官网 8 处同源（`index`×2／`zh`×2／`terms`§4／`refund`§1／`thank-you`×2） |
| 交付方式 | **Download** | 发含下载链接的邮件，零后端 |
| `successUrl` | `https://daybooksuite.com/thank-you.html?checkout={checkout_id}` | 不设则停在 Paddle 自带成功页 |

**Paddle 过审后**，改 `index.html` 与 `zh.html` 各一处 Buy 按钮：
- 把 `href="#buy"` 换成真实 Checkout 链接；
- 删去 `class` 里的 `disabled`、删去 `aria-disabled="true"`、删去 `data-placeholder="paddle"`。

---

## 四 · 上线自检（一条命令 · 上线前必跑）

```bash
cd ~/Desktop/daybooksuite-site && grep -rn "data-placeholder" *.html
```

- **有命中** → 占位还在 → **不可上线**；
- **无输出** → 已换真链 → 可放行。

顺带核付款回跳参数是否还在：

```bash
cd ~/Desktop/daybooksuite-site && grep -n "checkout" thank-you.html
```

---

## 五 · 追溯与回滚（入仓后新增）

- 本目录**已入版本管理**（`git` 基线提交 `cb5e0d2`）。
- 看改动：`cd ~/Desktop/daybooksuite-site && git log --oneline`
- 改坏了复原：`git checkout -- <文件名>`
- **规矩**：每次改动**先提交、后上线**——线上页面永远对应仓内某一个提交，可追溯。

---

## 六 · 定价改动口径（共 18 处同源 · 防漏）

现价 **US$10.99 一次性**（≥ $10，适配 Paddle 自助费率；$9.9／$1 均低于 Paddle 下限、须另谈）。动价时**官网 8 处**同改：
`index.html`×2（价格牌＋按钮）｜`zh.html`×2｜`terms.html` §4｜`refund.html` §1｜`thank-you.html`×2。
另**源码仓 10 处**：根 `README.md` 中英×2 ＋ `src/LICENSING.md`×2 → 改源后重打包同步三包 LICENSING。

---

*清单落盘：2026-10-12*
