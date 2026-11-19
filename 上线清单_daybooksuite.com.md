# daybooksuite.com 上线清单

> 一页看完。每步只需勾选；**放行硬闸**任一未过即**不得上线**。
> 细则与文件对照见 `README.md`；本页是**执行页**。

---

## 〇 · 放行硬闸（一票否决 · 上线前逐条核）

| # | 硬闸 | 怎么判（可核） | 现状 |
|---|---|---|---|
| **H0** | **部署通道**已通（改完能一键上线·非手工重传） | 见 §一b | ✅ **已通**（2026-10-15 实测：push 后线上自动更新） |
| **H1** | ~~Buy 按钮已换**真链**~~ | ~~`grep` 无 `data-placeholder`（见 §四）~~ | ⛔ **已废止**（v4.x 免费发行·无 Buy 按钮·无付费流程） |
| **H2** | ~~Paddle `successUrl` 指向本站付款后页~~ | ~~Paddle 后台已填（见 §三）~~ | ⛔ **已废止**（无付费流程） |
| **H3** | ~~Paddle 交付方式 = **Download**~~ | ~~Paddle 后台~~ | ⛔ **已废止**（无付费流程） |
| **H4** | 域名邮箱 `hi@daybooksuite.com` 已通 | 发一封、收回执 | 🟡 **MX 已指向 Cloudflare Email Routing**（route1/2/3.mx.cloudflare.net）·**待实发一封**确认落到 sina |
| **H5** | 正式域名 HTTPS 可访问 | 浏览器直访 `https://daybooksuite.com` | ✅ **线上 = v4.0.7**（与仓同步·2026-11-19 实测） |
| **H6** | 三政策页在线可达（Terms / Privacy / Refund） | 逐页打开 | ✅ 线上可达（均 200） |
| **H7** | 付款后落地页 `thank-you.html` 在线 | 直访 `/thank-you` | ✅ **线上可达（200）** |

> ~~**H1 与 H2 是同一件事的两面**：Buy 真链点了要能跳 Paddle 结账，付完要能回到本站 `thank-you`。**两条都过，才算付款闭环通。**~~
>
> **【2026-11-19 废止】** H1／H2／H3 属 v3.x 付费版硬闸；v4.x 改为免费直下，付款闭环整体不存在。后来者**不得**再照这三条判红。

---

## 〇b · 线上现状诊断（**2026-10-15 15:36 复测·前诊已消解**）

| 项 | 实测（2026-10-15 15:36） |
|---|---|
| 托管 | **Cloudflare Pages**（`server: cloudflare`·NS `angelina/cartman.ns.cloudflare.com`） |
| 首页 `/` | ✅ **v3.1**（规模 6,541 行·daybook 1,653 · mcp 2,962 · orch 1,926） |
| 中文页 `/zh` | ✅ **v3.1** |
| `/terms`、`/privacy`、`/refund` | ✅ 均 **200** |
| `/thank-you` | ✅ **200**（前诊「线上缺失」已消） |
| 任意不存在路径（如 `/__nope_xyz`） | ✅ **404**（**前诊 SPA 回退已消**） |
| 部署通道 | ✅ **push 即自动上线**（2026-10-15 推 `6289c41` 后线上自动翻 v3.1） |

> **前诊（2026-10-12）的两个洞——`thank-you` 缺失、SPA 回退——均已消除**；线上已与仓同步至 v3.1。
> 仍候 **Paddle 过审** 才能放行 H1–H3（付款闭环），与站点托管无关。

## 〇c · 2026-10-30 复测（**v3.2.2 上线**·老宋令「重新打包，GITHUB、daybooksuite.com、pypi.org」）

| 项 | 实测（2026-10-30） |
|---|---|
| 首页 `/` 版本 | ✅ **v3.2.2**（规模 7,735 行·daybook 1,992 ｜ mcp 3,548 ｜ orch 2,195） |
| 中文页 `/zh` 版本 | ✅ **v3.2.2** |
| 四条下载直链 | ✅ 均改指 `…/releases/download/v3.2.2/*.zip`（87.0／101.6／93.9／512.4 KB） |
| `thank-you.html` 的 release 标签 | ✅ v3.0 → **v3.2.2**（前次漏改·本次一并收） |
| PyPI | ✅ `daybook-suite`／`daybook-mcp`／`daybook-orch` **均 3.2.2**（后两件为首次上 PyPI） |
| 部署方式 | ✅ 仍是 **push 即上线**（Cloudflare Pages 连本仓·Git 集成） |

**本次同步内容（对应 daybook-suite v3.2.2）**：条目头「秒可选」＋**口径漂移闸**（`index/.head_stamp.json`
记口径指纹，口径一改即清索引全量重扫）＋ `mcp_tools.py` 缺省口径归一；并修 `test/run.sh`
Fortran 一步**吃 stdin 导致整条自测无限挂住**的病。

> **口径提示**：站点上的「当前版本」此前停在 v3.1、下载直链停在 v3.2.1 —— 属**发版后未回站同步**
> 的老病（与 H0 部署通道无关）。本次已一并收口。

## 〇d · 2026-10-30 再测（**v3.2.3 许可补记**·老宋「写进许可」）

| 项 | 实测（2026-10-30） |
|---|---|
| 首页／中文页版本 | ✅ **v3.2.3**（四条直链同步，均实测 200） |
| 新增「官方渠道」声明块 | ✅ 首页（英文）与中文页各一段：唯一官方发行名＝ PyPI 三发行名 + 本站 + 本仓；其余同名包均非官方 |
| PyPI | ✅ 三件均 **3.2.3**；三个 wheel 的 `METADATA` 均含「发行名归属」段 |
| 代码 | ✅ **零变更**（逐件 sha 与 v3.2.2 全同；唯 `mcp_server.py` 版本串随套件轴；构建号未变 1992／3548／2195） |

**为何单发一版**：v3.2.2 把 `daybook-mcp`／`daybook-orch` 两个 PyPI 名首次发布，但许可文本里
没写「名归谁、怎么判官方」——**占位不声明＝声明不落字**；而 PyPI 同版本号**不可重传**，
故只有新版本号能把许可文本真正落到 PyPI 项目页。许可证正文见仓内 `src/LICENSING.md` **§七**
（随三包发布）。

**处置（2026-10-12 20:00 · 老宋裁甲 · ✅ 2026-10-15 已落实）**：站点已**推 GitHub 仓** `songliansheng1972/daybooksuite-site`（main）；已建 **Cloudflare Pages 连本仓**——
- 两洞**已修**：`thank-you.html` 随仓部署在线（洞①消）；**无 SPA 回退**，`404.html` 生效（洞②消）。**均已 2026-10-15 15:36 实测确认**。
- 以后 **`git push` 即上线**，不再手工重传（**已实测：推 `6289c41` 后线上自动翻 v3.1**）。

---

## 一 · Cloudflare Pages 连本仓（Git 集成 · 甲方案）—— ✅ **已完成**（2026-10-15 实测生效）

> 以下为**一次性设置步骤留存**（已做完，存档备查）。仓：`https://github.com/songliansheng1972/daybooksuite-site`（main）。

1. **自己敲网址**登录 `dash.cloudflare.com`（**不点邮件里的链接**）。
2. 左侧 **Workers & Pages → Create → Pages → Connect to Git**。
3. 授权 GitHub，选仓 `songliansheng1972/daybooksuite-site`；**生产分支选 `main`**。
4. **构建设置**：Framework preset = **None**；Build command **留空**；Build output directory = **`/`**。
5. **Save and Deploy** → 得 `xxx.pages.dev` → **先直访验证**：首页、`/zh`、三政策页、`/thank-you.html`、随便一个不存在路径（应为 **404**）。
6. **Custom domains → 加 `daybooksuite.com`**：
   - 域名现绑在**旧项目**（Upload assets 建的）上——按提示**先在旧项目移除该域名**，再在**新项目**加入；如提示记录冲突，按提示修正即可（自动 HTTPS）。
7. 旧的 Upload 项目**弃用**（可留作回退，或删）。

**以后改法**：改本地 → `git commit` → `git push` ⇒ 自动上线。**不再手工拖拽。**

> 备用：GitHub Pages（仓 Settings → Pages → Source 选 `main` 根）。二者**均无需备案**。

---

## 一b · 部署通道（根治「改了本地不更新」· 三选一）

> **H0 硬闸**：只要还停在「手工拖拽上传」，每次改完都可能忘传——**这次就是这么漏的**。

- **✅ 甲（已选定 · ✅ 已落实）**：站点已入 GitHub 仓 → Cloudflare Pages **已连该仓** → `git push` 即自动上线。
  - 仓：`https://github.com/songliansheng1972/daybooksuite-site`（main）✅ **已推送**
  - ✅ **已完成**：CF 侧**连仓 + 域名指向**（2026-10-15 实测：推 `6289c41` 后线上自动翻 v3.1）。
  - 注：旧 Upload 项目**弃用**（不能改 Git 集成·须新建 Pages 项目·域名重绑——**已办**）。
- ~~乙（wrangler 直推）~~ · ~~丙（手工重传）~~：甲已选定，另两案不再执行。

**本次两处已随仓备妥**：①`thank-you.html` 已在仓内（部署即在）；②新增 `404.html`（不存在路径 → 404，不再回退首页）。

---

## 二 · 域名与邮箱（与 §一 并行）

- **DNS**：Cloudflare 会分配两个 NS 地址（形如 `xxx.ns.cloudflare.com`）→ 去**域名注册商**处改成这两个 → 等待生效（通常几分钟到几小时）。
- **域名邮箱**：Cloudflare 后台 **Email Routing** → 添加 `hi@daybooksuite.com` → 转发到你的 sina 邮箱。
- **邮件提醒**：Cloudflare 会发三类信（邮箱验证 / 改 NS / 站点已激活），发件人都是 `@cloudflare.com`。
  - 你贴的那段官网文案**不是**确认信（是首页宣传页）。
  - **一律不点邮件里的链接**，直接登录后台看状态。

---

## 三 · Paddle 侧（~~决定 Buy 能否放行~~ **已废止**）

> **【2026-11-19 废止】** 本站自 v4.x 起免费直下，站内无 Buy 按钮、无 Paddle 结账。本节仅为 v3.x 存档，**不要再照此操作**。

| 项 | 该设成 | 备注 |
|---|---|---|
| 商品价格 | **US$10.99 一次性** | 官网 8 处同源（`index`×2／`zh`×2／`terms`§4／`refund`§1／`thank-you`×2） |
| 交付方式 | **Download** | 发含下载链接的邮件，零后端 |
| `successUrl` | `https://daybooksuite.com/thank-you?checkout={checkout_id}` | 不设则停在 Paddle 自带成功页 |

**`successUrl` 设在哪（2026-10-12 亲核 Paddle 文档）**：Paddle Billing **后台并没有 successUrl 字段**，它是在打开结账时由**代码**传入：

```js
Paddle.Checkout.open({
  items: [{ priceId: 'pri_xxx', quantity: 1 }],
  settings: { successUrl: 'https://daybooksuite.com/thank-you?checkout={checkout_id}' }
});
```

省略则回退到后台 **Checkout › Checkout Settings › Default payment link**（沙盒：`sandbox-vendors.paddle.com/checkout-settings`；正式：`vendors.paddle.com/checkout-settings`）——但那是**收款落地页**（要含 Paddle.js），不是感谢页，语义不对 ⇒ **必须显式传 `successUrl`**。占位符 `{checkout_id}` 正好匹配 `thank-you.html` 读取的 `?checkout=` 参数，无需改页面。

**路径口径（2026-10-12 统一 · 全站已改）**：CF Pages 默认把 `*.html` **308 去后缀**（`/terms.html` → `/terms`），故站内链接与 `successUrl` **一律用无后缀形式**（`/thank-you`，不是 `/thank-you.html`），否则每点一次导航都白吃一跳 308。已改 6 个页面共 51 处，`grep -rn 'href="[^"]*\.html"' *.html` 应**无输出**。

**Paddle 过审后**，改 `index.html` 与 `zh.html` 各一处 Buy 按钮：
- 把 `href="#buy"` 换成真实 Checkout 链接；
- 删去 `class` 里的 `disabled`、删去 `aria-disabled="true"`、删去 `data-placeholder="paddle"`。

---

## 四 · 上线自检（~~一条命令·上线前必跑~~ **已废止**）

> **【2026-11-19 废止】** 该自检用于核「Paddle 占位是否已换真链」；v4.x 已无 Buy 按钮，`data-placeholder` 在整站为 0，本节失去判别意义。
> 现行自检改为：`grep -c "4\.0\.[5-6]" *.html` 应为 **0**（版本串不得残留旧版），并逐条 `curl -I` 四条下载直链须 **200**。

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

- 本目录**已入版本管理并已推远端**：`https://github.com/songliansheng1972/daybooksuite-site`
- 看改动：`cd ~/Desktop/daybooksuite-site && git log --oneline`
- 改坏了复原：`git checkout -- <文件名>`
- **规矩**：每次改动**先提交、再 push**——线上页面永远对应仓内某一个提交，可追溯；Git 集成后 push 即自动上线。

---

## 六 · 定价改动口径（~~共 18 处同源 · 防漏~~ **已废止**）

> **【2026-11-19 废止】** 现价 **$0 免费**，无付费流程；「官网 8 处 + 源码仓 10 处 = 18 处同源」的改价口径不再适用。

现价 **US$10.99 一次性**（≥ $10，适配 Paddle 自助费率；$9.9／$1 均低于 Paddle 下限、须另谈）。动价时**官网 8 处**同改：
`index.html`×2（价格牌＋按钮）｜`zh.html`×2｜`terms.html` §4｜`refund.html` §1｜`thank-you.html`×2。
另**源码仓 10 处**：根 `README.md` 中英×2 ＋ `src/LICENSING.md`×2 → 改源后重打包同步三包 LICENSING。

---

*清单落盘：2026-10-12*

---

## 〇d · 2026-10-30 再测（**v3.2.4 上线**·老宋裁「把『发行名』也写进商标申请范围——同意」）

| 项 | 实测（2026-10-30） |
|---|---|
| 首页 `/` 版本 | ✅ **v3.2.4**（直链 92.4／107.0／99.4／535.1 KB） |
| 中文页 `/zh` 版本 | ✅ **v3.2.4** |
| 新增商标声明块 | ✅ 首页 `Trademark.` ／ 中文页 `商标（名不可乱用）。` 各一处 |
| `thank-you.html` 的 release 标签 | ✅ v3.2.3 → **v3.2.4** |

> **商标块与仓内 `src/TRADEMARK.md` 同源**：三个发行名 `daybook-suite`／`daybook-mcp`／`daybook-orch`
> 写入申请范围（第 9 类·可下载软件；第 42 类·软件开发与平台服务）。
> ⚠️ 状态如实：**拟申请、尚未提交、尚未核准**；核准前标 ™ 不标 ®。站上三处（首页／中文页／本清单）口径一致。

---

## 〇e · 2026-11-19 复测（**v4.0.7 上线**·站点版本轴追到 4.0.7）

| 项 | 实测（2026-11-19） |
|---|---|
| Release `v4.0.7` 附件 | ✅ 4 件，名与字节数亲核一致：`daybook-v4.0.7.zip` 311,008｜`daybook-mcp-v4.0.7.zip` 259,803｜`daybook-orch-v4.0.7.zip` 215,569｜`daybook-suite-v4.0.7.zip` 786,336 |
| 首页 `/` 版本 | ✅ **v4.0.7**（规模 17,168 行·daybook 5,138 ｜ mcp 6,939 ｜ orch 5,091，取自 release 自证读数） |
| 中文页 `/zh` 版本 | ✅ **v4.0.7** |
| 四条下载直链 | ✅ 均改指 `…/releases/download/v4.0.7/*`，`curl -I` 逐条 **200** 且 `content-length` 与附件字节数一致 |
| 站上按钮体积标注 | ✅ 303.7／253.7／210.5／767.9 KB（bytes/1024 取一位小数，与 4.0.5 时口径相同） |
| `thank-you.html` 的 release 标签 | ✅ v4.0.5 → **v4.0.7** |
| 发行模式 | ✅ **免费直下**（$0）；无 Buy 按钮、无 Paddle；PyPI 三发行名同步在发 4.0.7 |

> 口径提醒：4.0.7 的「构建号」较 4.0.5（2,197／4,319／2,755）几乎翻倍，是因发行包新增 `signer/` 与 `setup/` 两目录
> 随包（见 release 说明「随包与权限」段）。站上「规模」一栏由此同步改写，非笔误。

