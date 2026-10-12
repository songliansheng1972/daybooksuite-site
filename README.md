# daybooksuite.com — 落地页与政策三页

Paddle 审核要求自有域名站，且**必须含 Terms of Service 与 Privacy Policy**（本目录已备**退款政策**）。
本目录是**静态站**，无依赖、无构建。

## 文件

| 文件 | 用途 |
|---|---|
| `index.html` | 落地页（英文·是什么 / 三件包 / 定价 $9.9 / 联系） |
| `zh.html` | 落地页中文版（语言切在导航「中文 / English」） |
| `terms.html` | 服务条款（含 Paddle 为 Merchant of Record 的说明） |
| `privacy.html` | 隐私政策（本地纯文本·无遥测；购买数据由 Paddle 收集） |
| `refund.html` | 退款政策（14 天） |
| `thank-you.html` | **付款后落地页**（Paddle `successUrl` 指向此页；取件指引 + 许可范围 + 更新与退款；`noindex`） |
| `style.css` | 共用样式（纸感/账页风格） |

## 本地预览

```bash
cd ~/Desktop/daybooksuite-site
python3 -m http.server 8080
# 浏览器打开 http://localhost:8080
```

## 上线（二选一）

**甲 · Cloudflare Pages（推荐——与域名/邮箱同一后台）**
1. Cloudflare 控制台 → Workers & Pages → Create → Pages → Upload assets
2. 上传本目录全部文件 → 得到 `xxx.pages.dev`
3. Pages → Custom domains → 绑 `daybooksuite.com`（自动 HTTPS）

**乙 · GitHub Pages**
1. 把本目录内容推到某仓（如 `daybook-suite/site`）
2. 仓 Settings → Pages → Source 选该目录
3. 自定义域名填 `daybooksuite.com` + DNS 加 CNAME

## 待办（发布前）

- [ ] `index.html` / `zh.html` 的 `Buy` 按钮：Paddle 审核通过后，换成真实 Checkout 链接（现为占位 `#buy`）
- [ ] **Paddle 后台设 `successUrl`** = `https://daybooksuite.com/thank-you.html?checkout={checkout_id}`（或 Checkout settings 里的 post-checkout 跳转）；不设则停在 Paddle 自带成功页
- [ ] Paddle 商品交付方式选 **Download**（发含下载链接的邮件），零后端
- [ ] 域名邮箱 `hi@daybooksuite.com`：Cloudflare Email Routing 转发到 sina 邮箱后即生效
- [ ] 备案：用 Cloudflare Pages / GitHub Pages 托管，**无需备案**
- [ ] 定价：现为 **$9.9 一次性**（个人非商业）；如改价，`index.html`／`zh.html`／`terms.html` §4／`refund.html` §1 四处同改
