# 📊 Static Crypto Portfolio

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
  <img src="https://img.shields.io/badge/Stack-Vue.js%203-42b883.svg" alt="Vue">
  <img src="https://img.shields.io/badge/Data-Local_Storage-blue.svg" alt="Local Storage">
  <br><br>
  <!-- Demo 按钮 -->
  <a href="https://static-crypto-portfolio.pages.dev/" target="_blank">
    <img src="https://img.shields.io/badge/🚀_Live_Demo-在线演示-blue?style=for-the-badge&logo=google-chrome&logoColor=white" height="36">
  </a>
  <br>
  <!-- 👇 这就是你要加的那句话，用灰色小字显示显得很精致 -->
  <sub style="color: gray;">
    Don't want to deploy? You can use the demo directly. Your data stays local.
    <br>
    不想自己部署？可以直接使用上面的在线演示。数据仅在本地，安全无忧。
  </sub>
</p>

<p align="center">
  <strong>A privacy-first, serverless cryptocurrency portfolio tracker.</strong>
  <br>
  无需后端、隐私优先、基于浏览器的纯静态加密资产追踪器。
</p>

<p align="center">
  <a href="#-key-features">Key Features</a> •
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-configuration">Configuration</a> •
  <a href="#-license">License</a>
</p>

---

## ✨ Key Features (功能亮点)

### 🛡️ Privacy & Security (隐私安全)
*   **100% Client-Side**: No server, no database, no cookies. Your financial data never leaves your device unless you choose to sync it.
*   **Local Storage**: All transaction data is stored in your browser's `localStorage`.
*   **纯本地化**: 没有任何后台服务器，数据完全存储在你的浏览器本地，绝对安全。

### 🚀 Lightweight & Portable (轻量便携)
*   **Single HTML File**: The entire app is just one `.html` file. You can download it and run it offline.
*   **Responsive Design**: Optimized for Desktop, Tablet, and Mobile.
*   **单页应用**: 只有一个 HTML 文件，部署简单，甚至可以离线运行。适配手机和电脑端。

### 📈 Comprehensive Tracking (全能记账)
*   **Multiple Transaction Types**: Support for Buy (入账), Sell (出账), Transfer (划转), Swap (兑换), and Reconcile (余额校准).
*   **Rich Analytics**: Automatic calculation of **Average Cost**, **PnL** (Unrealized Profit/Loss), and **Holdings Distribution**.
*   **多维分析**: 自动计算平均持仓成本、总盈亏、持仓分布。支持记录交易、转账、兑换及空投/利息校准。

### ☁️ Sync & Integration (同步与扩展)
*   **WebDAV Sync**: Optional support for WebDAV to sync data across devices (e.g., via Nextcloud, Jianguoyun).
*   **Data Portability**: Full JSON/CSV Import and Export support.
*   **Real-time Prices**: Fetch real-time prices via a customizable Cloudflare Worker proxy (supports MEXC/Binance data).
*   **多端同步**: 支持配置 WebDAV 实现多设备数据同步。支持 CSV/JSON 导入导出。支持自定义代理获取实时币价。

---

## 🚀 Quick Start (快速开始)

### Option 1: Cloudflare Pages (Recommended)
1. Fork this repository.
2. Go to [Cloudflare Dashboard](https://dash.cloudflare.com/) > **Pages**.
3. Connect your GitHub account and select this repository.
4. **Build settings**: Leave everything empty (No framework, no build command).
5. Click **Deploy**. Done!

### Option 2: Run Locally
1. Download the `index.html` file from this repository.
2. Open it with any web browser (Chrome, Safari, Edge).
3. Start tracking!

---

## ⚙️ Configuration (配置指南)

### 💹 Real-time Price Setup (实时价格配置)
Due to browser CORS policies, you need a simple proxy to fetch prices from exchanges like MEXC.
由于浏览器的跨域限制，你需要配置一个简单的 Cloudflare Worker 来获取实时价格。

1. **Create a Cloudflare Worker** and paste the code below:
   **创建 Cloudflare Worker** 并粘贴以下代码：

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
```
Deploy the worker and copy its URL (e.g., https://price-proxy.yourname.workers.dev).
部署 Worker 并复制生成的 URL。
Configure in App: Click the Settings (⚙️) button in the top-right corner of the Portfolio app, paste your URL, and save.
应用内配置：点击网页右上角的设置按钮，填入你的 Worker URL 并保存。




网站预览展示：
添加询价API： <img width="1324" height="1086" alt="image" src="https://github.com/user-attachments/assets/8be91fce-ca99-4281-9128-2252789cfff3" />
可按照【币种】汇总各个平台账户总的余额和估值：<img width="1309" height="1217" alt="image" src="https://github.com/user-attachments/assets/99bf1274-3b85-44a7-935b-f4c4f6421936" />
可按照【币种-平台-账户】汇总分开查看：<img width="1310" height="1218" alt="image" src="https://github.com/user-attachments/assets/86efbb4f-6c1b-46a6-bde7-d04be97c694a" />
可按照【平台-账户】汇总查看，方便查看某个账户的持仓情况：<img width="1315" height="1219" alt="image" src="https://github.com/user-attachments/assets/e13aa30b-d080-4a93-b9e0-1b96aefffa3b" />

📄 License
This project is licensed under the MIT License.
Feel free to fork, modify, and use it for your personal finance.
