# static-crypto-portfolio
A privacy-first, single-page cryptocurrency portfolio tracker. No server required, runs entirely in your browser with LocalStorage.

Features (功能特性):
🛡️ Privacy Focused: Data stored in localStorage.
⚡ Serverless: Single HTML file, no backend needed.
☁️ Cloud Sync: Optional WebDAV support.
💹 Real-time Prices: Custom Cloudflare Worker proxy support.

Quick Start (快速开始):
下载 index.html 直接打开就能用，也可以部署到自己的Cloudflare Pages中

DEMO：
Cloudflare Pages 演示链接：https://static-crypto-portfolio.pages.dev/
不愿意自己部署Pages的，也可以直接使用DEMO，同样的DEMO的数据也是本地的。

📊 实时价格配置 (Real-time Price Setup)

由于浏览器的安全策略 (CORS)，静态网页无法直接访问交易所 API。为了获取实时价格，需要部署一个免费的 Cloudflare Worker 作为代理。

步骤 1：部署 Cloudflare Worker

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)。
2. 进入 Workers & Pages -> Create Application -> Create Worker。
3. 点击 Deploy，然后点击 Edit code。
4. 将 `worker.js` 中的代码替换为以下内容：

```javascript
export default {
  async fetch(request, env, ctx) {
    const url = "https://api.mexc.com/api/v3/ticker/price";

    try {
      const response = await fetch(url, {
        headers: {
          'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36'
        }
      });

      // 2. 检查是否成功，不成功则返回错误信息
      if (!response.ok) {
         return new Response("Source API Error: " + response.status, { status: 500 });
      }

      // 3. 创建新响应，加上跨域头 (CORS)
      const newRes = new Response(response.body, response);
      newRes.headers.set("Access-Control-Allow-Origin", "*");
      newRes.headers.set("Access-Control-Allow-Methods", "GET, HEAD, POST, OPTIONS");
      newRes.headers.set("Content-Type", "application/json");

      return newRes;

    } catch (err) {
      return new Response("Worker Error: " + err.message, { status: 500 });
    }
  },
};
