# 🚀 自动聚合免费节点订阅

本仓库致力于自动化抓取、筛选并聚合各类免费代理节点，提供稳定、便捷的节点订阅服务。

## 📋 功能特点
* **自动化抓取**：基于 GitHub Actions 实现定时全自动更新，无需手动维护。
* **智能筛选**：自动剔除包含广告、已失效及异常的垃圾节点。
* **统一重命名**：节点自动重命名为“科技共享节点分享”，统一管理，清爽整洁。
* **全链路加速**：前端集成 Cloudflare Worker 作为 CDN 加速节点，有效规避 GitHub Raw 频繁限流问题。

## ☁️ Cloudflare Worker 部署教程
如果您也想拥有自己的高速订阅通道，可以按照以下步骤部署：

1. **注册/登录**：访问 [Cloudflare 官网](https://dash.cloudflare.com/) 并登录账号。
2. **创建 Worker**：
   - 在左侧菜单点击 **Workers & Pages** -> **Create** -> **Create Worker**。
   - 给 Worker 起个名字，点击 **Deploy**。
3. **编辑代码**：
   - 点击 **Edit code**。
   - 将下方的代码模板复制并覆盖到编辑器中（**请务必将代码内的 `GITHUB_RAW_URL` 替换为您自己的 GitHub 原始文件链接**）。
   - 点击右上角的 **Deploy** 保存。
4. **获取域名**：
   - 部署完成后，您会得到一个 `xxx.workers.dev` 的域名，这就是您的专属订阅加速地址。
     
# 🛡️ 开源声明与协议

**免费开源**：本项目遵循 **MIT License** 协议，允许个人进行学习、研究与使用。

**禁止商业**：严禁将本项目代码、抓取脚本或生成的订阅链接用于任何商业用途（包括但不限于倒卖节点、通过节点流量盈利、开设收费机场等）。

**免责声明**：本项目仅供技术交流使用。节点来源均为互联网公开资源，本人不对节点的有效性、安全性及由此引发的任何法律问题负责。请用户在使用后 24 小时内自行删除。

感谢对本项目的关注。若您觉得有用，欢迎给个 **Star** 支持！


### 📄 Worker 代码模板
```javascript
export default {
  async fetch(request, env) {
    // 替换为您的 GitHub Raw 链接
    const GITHUB_RAW_URL = "https://raw.githubusercontent.com/tonyhuang23-sys/CFnodes/main/output/sub.txt";
    
    try {
      const response = await fetch(GITHUB_RAW_URL, {
        headers: { "User-Agent": "Mozilla/5.0" }
      });
      const newResponse = new Response(response.body, response);
      newResponse.headers.set('Access-Control-Allow-Origin', '*');
      newResponse.headers.set('Content-Type', 'text/plain; charset=utf-8');
      return newResponse;
    } catch (e) {
      return new Response("Error: 无法连接到 GitHub", { status: 500 });
    }
  }
};
