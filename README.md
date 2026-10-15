# daybooksuite.com — 落地页与政策三页

Paddle 审核要求自有域名站，且**必须含 Terms of Service 与 Privacy Policy**（本目录已备**退款政策**）。
本目录是**静态站**，无依赖、无构建。

## 文件

| 文件 | 用途 |
|---|---|
| `index.html` | 落地页（英文·是什么 / 三件包 / 定价 $10.99 / 规模 6,541 行 / 联系） |
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

## 待办（发布前）

> 逐步操作与**放行硬闸**见同目录《`上线清单_daybooksuite.com.md`》（执行页）。

- [ ] `index.html` / `zh.html` 的 `Buy` 按钮：Paddle 审核通过后，换成真实 Checkout 链接（现为占位 `#buy`）
- [ ] **Paddle 后台设 `successUrl`** = `https://daybooksuite.com/thank-you.html?checkout={checkout_id}`（或 Checkout settings 里的 post-checkout 跳转）；不设则停在 Paddle 自带成功页
- [ ] Paddle 商品交付方式选 **Download**（发含下载链接的邮件），零后端
- [ ] 域名邮箱 `hi@daybooksuite.com`：Cloudflare Email Routing 转发到 sina 邮箱后即生效
- [ ] 备案：用 Cloudflare Pages / GitHub Pages 托管，**无需备案**
- [ ] 定价：现为 **US$10.99 一次性**（个人非商业；≥$10 适配 Paddle 自助费率）；如改价，**官网 8 处**同改：`index.html`×2（价格牌＋按钮）／`zh.html`×2／`terms.html` §4／`refund.html` §1／`thank-you.html`×2；另**源码仓 10 处**（根 `README.md` 中英×2 ＋ `src/LICENSING.md`×2 → 重打包同步三包）——共 **18 处同源**
