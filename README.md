# EACO X402 Protocol — 快速概览 | Quick Overview

> 🪙 **HTTP 402 状态码等了三十年，终于在 Solana + EACO 上被激活**
> The HTTP 402 status code waited 30 years — now activated on Solana + EACO

---

## 核心概念 | Core Concept

```
EACO X402 = HTTP 402 + Solana + EACO Token

AI Agent 请求 API
    ↓
API 返回 402 Payment Required + 支付条款
    ↓
Agent 使用 EACO 代币完成链上支付
    ↓
携带签名重试 → 获取数据 ✅
```

---

## 三种结算模式 | 3 Settlement Modes

| 模式 | 适用场景 | 结算速度 | EACO 定价参考 |
|------|---------|---------|--------------|
| **按次付费** (Per-Call) | AI 推理、数据查询 | 即时 (~400ms) | 0.001-1 EACO/次 |
| **时段订阅** (Subscription) | 高频行情推送 | 订阅期内 Bearer Token | 10 EACO/月 |
| **托管结算** (Escrow) | 高价值数据集、AI 模型 | Oracle 验证后释放 | 100-10000+ EACO |

---

## 支持的语言 | Supported Languages

🌐 **联合国 6 大官方语言 + 8 种区域语言 = 14 种语言全覆盖**

| 类别 | 语言 |
|------|------|
| 🇨🇳🇹🇼 中文 | 简体中文 · 繁体中文 |
| 🌍 联合国官方 | English · Français · العربية · Русский · Español |
| 🌏 东南亚 | Bahasa Melayu · Tiếng Việt · ภาษาไทย |
| 🌏 东亚 | 日本語 · 한국어 |
| 🌐 欧洲补充 | Português · Deutsch |

---

## EACO 代币信息 | Token Info

| 参数 | 值 |
|------|------|
| **代币名称** | EACO (Earth's Best Coin) |
| **合约地址** | `DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH` |
| **总供应量** | 1,350,000,000 EACO |
| **区块链** | Solana (SPL Token) |
| **区块浏览器** | [Solscan](https://solscan.io/token/DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH) |

---

## 核心公式 | Core Formula

```
EACO = Energy × Attitude × Cooperation × Optimization
     = 正能量 × 积极态度 × 全球协作 × 持续优化

目标：
  提升个人认知
  提升组织效率
  提升社会文明
  提升地球可持续发展
  与 100 个认知模型结合
```

---

## EACO X402 适用人群 | Who Uses This?

| 角色 | 用法 |
|------|------|
| 🤖 AI Agent 开发者 | 用 EACO 支付 API 调用费用，自主完成商业闭环 |
| 🌐 API 提供商 | 接入 EACO X402 网关，将服务变现为 EACO/USDC |
| 💼 EACO 社区成员 | 使用 EACO 购买 AI 服务，参与 Agent World 生态 |
| 🏛️ DAO 治理者 | 投票决定协议参数、费用率、升级提案 |

---

## 快速链接 | Quick Links

| 资源 | 链接 |
|------|------|
| 📄 完整协议文档 | `./PROTOCOL.md` |
| 🌐 多语言协议文档 | `./PROTOCOL_MULTILINGUAL.md` |
| 🔗 EACO 官网 | https://eaco-build-world.base44.app |
| 📖 x402 规范 | https://docs.ipay.sh |
| 🔍 市场注册表 | https://registry.pr402.org |
| 📊 链上数据 | https://orbmarkets.io/token/DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH |

---

## 文件结构 | File Structure

```
eaco-x402/
├── PROTOCOL.md              ← 完整协议规范（英文主版）
├── PROTOCOL_MULTILINGUAL.md ← 多语言完整协议文档
└── README.md                ← 本文件：快速概览
```

---

*🌌 EACO — 从地球走向银河的价值基础设施*
*🌌 EACO — Building Value Infrastructure from Earth to the Galaxy*
