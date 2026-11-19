# daybooksuite.com — 落地页与政策三页

本站早期为 Paddle 审核而建（要求自有域名 + Terms/Privacy），该审核随 v4.x 免费发行**已废止**；
三政策页仍保留并在线（合规与透明度需要）。本目录是**静态站**，无依赖、无构建。

## 文件

| 文件 | 用途 |
|---|---|
| `index.html` | 落地页（英文·是什么 / 三件包 / 免费直下 / 规模 17,168 行 / 联系） |
| `zh.html` | 落地页中文版（语言切在导航「中文 / English」） |
| `terms.html` | 服务条款（含 Paddle 为 Merchant of Record 的说明） |
| `privacy.html` | 隐私政策（本地纯文本·无遥测；购买数据由 Paddle 收集） |
| `refund.html` | 退款政策（14 天） |
| `thank-you.html` | **付款后落地页**（Paddle `successUrl` 指向此页；取件指引 + 许可范围 + 更新与退款；`noindex`） |
| `404.html` | 自定义 404 页（不存在路径 → **404**，不再回退首页） |
| `style.css` | 共用样式（纸感/账页风格） |

## 本地预览

```bash
cd ~/Desktop/daybooksuite-site
python3 -m http.server 8080
# 浏览器打开 http://localhost:8080
```

## 上线（部署方式：Cloudflare Pages 连本仓 · Git 集成）

本目录**已是 Git 仓并已推远端**：

- 仓：`https://github.com/songliansheng1972/daybooksuite-site`
- **以后改法**：改本地 → `git commit` → `git push` ⇒ Cloudflare 自动构建上线（**不再手工拖拽**）。

**首次在 Cloudflare 侧连仓（一次性）**

1. 自己敲网址登录 `dash.cloudflare.com` → **Workers & Pages → Create → Pages → Connect to Git**。
2. 授权 GitHub，选仓 `songliansheng1972/daybooksuite-site`，**生产分支 `main`**。
3. **构建设置**：Framework preset = **None**；Build command **留空**；Build output directory = **`/`**。
4. **Save and Deploy** → 得 `xxx.pages.dev` → 先直访验证各页（含 `/thank-you.html` 与 404）。
5. **Custom domains** → 加 `daybooksuite.com`（若提示记录冲突，按提示修正）。
6. ⚠ **不要**开启 Single Page Application（SPA）回退——保持不存在路径返回 404。

> 早前线上是「Upload assets」手工上传的旧项目（改了本地不会自动更新）。改建 Git 集成后**弃用旧项目**。
> 备用：GitHub Pages（仓 Settings → Pages → Source 选 `main` 根目录）。二者**均无需备案**。

## 现状（2026-11-19 · v4.0.7）

- **免费直下**：站上四条链接直指 GitHub Releases `v4.0.7` 的四个 zip（`daybook` / `daybook-mcp` / `daybook-orch` / `daybook-suite`），无账号、无支付；亦可 `pip install` 三个发行名。
- **无付费流程**：v4.x 起改为免费发行，站内**没有** Buy 按钮、没有 Paddle 结账、`thank-you.html` 只留「取件指引」。
- **待确认**：域名邮箱 `hi@daybooksuite.com`（Cloudflare Email Routing → sina）实发一封是否落地。

## 已废止（历史条目，勿再照此判红）

> 以下为 **v3.x 付费版**时期的待办，v4.x 免费发行后**整体不再适用**，仅存档备查。

- ~~`index.html` / `zh.html` 的 `Buy` 按钮：Paddle 审核通过后换成真实 Checkout 链接（占位 `#buy`）~~ —— 按钮已随免费化移除。
- ~~Paddle 后台设 `successUrl` / 交付方式选 Download~~ —— 无付费流程，无需设。
- ~~定价 US$10.99 一次性 · 官网 8 处同源~~ —— 现价为 **$0 免费**，18 处同源的口径作废。
