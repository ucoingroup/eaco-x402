# EACO X402 Protocol Specification
## 智能体经济原生支付协议 | EACO Native Payment Protocol for the Agent Economy
**Version 1.0 | 2026-07-05**
**Contract:** `DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH` (Solana SPL)
**Reference x402:** https://docs.ipay.sh | https://registry.pr402.org

---

## 目录 | Table of Contents

1. [简介 | Introduction](#1-introduction)
2. [EACO X402 概述 | Protocol Overview](#2-protocol-overview)
3. [自动售货机模型 | Vending Machine Model](#3-vending-machine-model)
4. [三种结算模式 | Three Settlement Modes](#4-three-settlement-modes)
5. [EACO 代币经济 | Token Economics](#5-token-economics)
6. [多语言支持 | Multilingual Support](#6-multilingual-support)
7. [开发者入口 | Developer Entry](#7-developer-entry)
8. [智能体的一天 | A Day in the Life of an Agent](#8-a-day-in-the-life-of-an-agent)
9. [安全与风险管理 | Security & Risk Management](#9-security--risk-management)
10. [治理与升级 | Governance & Upgrades](#10-governance--upgrades)
11. [附录 | Appendix](#11-appendix)

---

## 1. Introduction | 简介

### 1.1 背景 | Background

HTTP 402 Payment Required — 在 1997 年的协议规范中，设计者为机器间的原生支付预留了这个状态码。近三十年后，随着自主运行的 AI 智能体（Autonomous Agents）在网络上高频交易数据、算力与服务，这一愿景终于照进现实。

HTTP 402 was reserved in the 1997 HTTP specification as a payment-required signal, awaiting machine-native micropayments. Decades later, with autonomous AI agents conducting high-frequency transactions for data, compute, and services, this vision has finally arrived.

**EACO X402** 将 HTTP 402 状态码与 EACO 代币生态深度融合，构建专为 AI 智能体经济设计的原生支付结算层。EACO（Earth's Best Coin）= **E**nergy × **A**ttitude × **C**ooperation × **O**ptimization，为地球人民计算劳动价值、量化地球资源。

**EACO X402** fuses the HTTP 402 handshake with the EACO token ecosystem, building a native payment settlement layer purpose-built for the AI agent economy. EACO (Earth's Best Coin) = Energy × Attitude × Cooperation × Optimization — computing labor value and quantifying Earth's resources for every person on the planet.

### 1.2 核心价值主张 | Core Value Proposition

| 维度 | EACO X402 贡献 |
|------|---------------|
| **即时结算** Instant Settlement | Solana ~400ms 最终确认，EACO 微支付无摩擦 |
| **智能体原生** Agent-Native | AI 自主协商支付，无需人工干预 |
| **多语言覆盖** Multilingual | 12+ 语言支持，覆盖全球主要市场 |
| **多模式变现** Multi-Mode | 按次/订阅/托管三模式覆盖所有商业场景 |
| **文明叙事** Civilization Narrative | 从地球走向银河的价值基础设施 |

---

## 2. Protocol Overview | 协议概述

### 2.1 协议架构 | Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    AI Agent (EACO Wallet)                   │
│         EACO CA: DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH  │
└────────────────────────────┬────────────────────────────────┘
                             │ HTTP Request
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                     API Service (Seller)                     │
│           Returns: 402 + Payment Terms (JSON)               │
└────────────────────────────┬────────────────────────────────┘
                             │ 402 Response
                             ▼
┌─────────────────────────────────────────────────────────────┐
│              EACO X402 Gateway (Solana pr402)               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │ Per-Call    │  │ Subscription │  │ Escrow       │       │
│  │ Payout      │  │ Manager      │  │ (sla-escrow) │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
│                      │                                      │
│                      ▼                                      │
│  ┌──────────────────────────────────────────────────────┐   │
│  │      Solana Blockchain (SPL Token / EACO)            │   │
│  │  Settlement: USDC + EACO dual-token settlement        │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 HTTP 402 工作流 | HTTP 402 Workflow

```
Step 1: Agent ──HTTP Request──▶ API Server
Step 2: Agent ◀──402 + Terms─── API Server
Step 3: Agent ──EACO/USDC TX───▶ Solana (via X402 Gateway)
Step 4: Agent ──Signed TX───▶ API Server (Retry with signature)
Step 5: Agent ◀──200 OK + Data─── API Server ✓
```

### 2.3 支付条款格式 | Payment Terms Format

```json
{
  "required_payment": {
    "amount": "1000000",          // lamports / microlamports
    "mint": "DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH", // EACO
    "recipient": "EACO Treasury Address",
    "label": "AI Inference Credit",
    "description": "1000 tokens inference on EACO AI Gateway"
  },
  "mode": "per_call",             // per_call | subscription | escrow
  "id": "req_uuid_12345",
  "expires_at": 1751810400,       // Unix timestamp
  "languages": ["en", "zh", "ms", "vi", "ja", "ko"]
}
```

---

## 3. Vending Machine Model | 自动售货机模型

EACO X402 的运作逻辑可比作一台自动售货机：

| 自动售货机步骤 | EACO X402 对应 | 说明 |
|---------------|---------------|------|
| **按按钮** | Agent 发送 HTTP 请求 | 请求受保护的 premium 接口 |
| **读取标价** | 服务器返回 402 + 支付条款 | 详细列出金额、资产 mint、收款地址 |
| **投币结算** | Agent 通过 EACO X402 网关完成链上支付 | EACO 或 USDC 实时结算 |
| **出货数据** | Agent 携带交易签名重试，服务器验证后供应数据 | 完整闭环 |

### EACO X402 网关角色

EACO X402 网关扮演**投币器 + 结算引擎**双重角色：
- 处理所有链上复杂逻辑（签名验证、Token 转账、分配结算）
- 兼容 EACO 原生支付 + USDC 双通道结算
- 支持多语言支付确认消息
- 提供 EACO Agent World 8 大区域（全球/欧洲/亚洲/美洲/MENA/东南亚/南亚/东亚）的本地化合规接口

---

## 4. Three Settlement Modes | 三种结算模式

### 4.1 模式一：按次付费 | Per-Call Payouts

**适用场景：** 单次推理、数据查询、AI 问答、无状态微服务

**运作流程：**
```
每次请求 → 支付 EACO (0.001-1 EACO) → 实时结算 → 即时交付
```

**参考定价：**
- AI 推理: 0.01 EACO / 1K tokens
- 风险评估: 0.005 EACO / 查询
- 内容生成: 0.1 EACO / 篇

**技术实现：**
```json
{
  "mode": "per_call",
  "amount": 100000,        // microlamports
  "settlement": "instant",
  "token": "EACO"
}
```

---

### 4.2 模式二：时段订阅 | Time-Window Subscriptions

**适用场景：** 高频行情推送、持续数据 feeds、Agent World 8 区域专属 API

**运作流程：**
```
订阅请求 → 支付 EACO/USDC → 领取 JWT Token → 订阅窗口内 Bearer Token 访问
```

**订阅层级：**
| 等级 | 价格 | 有效期 | 适用 Agent |
|------|------|--------|-----------|
| Free | 0 EACO | 100 req/day | 探索用户 |
| Pro | 10 EACO/mo | 10,000 req/day | 独立 Agent |
| Enterprise | 100 EACO/mo | 无限制 | 企业级 Agent |

**技术实现：**
```json
{
  "mode": "subscription",
  "token_type": "JWT",
  "duration_seconds": 3600,
  "scopes": ["region:europe", "tier:pro"],
  "recharge_token": "EACO"
}
```

---

### 4.3 模式三：托管结算 | Escrow & Verified Delivery

**适用场景：** 高价值数据集、AI 模型权重、自定义任务、安全托管交易

**运作流程：**
```
买家锁入 EACO → 卖家交付 + 提交 Hash 证明 → Oracle 验证 → 资金释放或退款
```

**典型用例：**
- EACO Agent World: 区域 Agent 注册费托管
- AI 模型交易: 权重文件交付验证
- 数据集购买: 哈希校验 + 内容完整性证明

**技术实现：**
```json
{
  "mode": "escrow",
  "escrow_address": "sla-escrow PDA",
  "amount": "1000000000",  // EACO microlamports
  "oracle_program": "EACO Oracle Registry",
  "evidence_hash": "sha256:abc123...",
  "timeout_seconds": 86400
}
```

---

## 5. EACO Token Economics | 代币经济

### 5.1 核心参数 | Core Parameters

| 参数 | 值 |
|------|---|
| **合约地址** | `DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH` |
| **总供应量** | 1,350,000,000 EACO |
| **代币标准** | SPL Token (Solana) |
| **区块浏览器** | [Solscan](https://solscan.io/token/DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH) / [OKLink](https://www.oklink.com/solana/token/DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH) |

### 5.2 生态分配 | Ecosystem Allocation

| 板块 | 数量 | 占比 | X402 支付场景 |
|------|------|------|-------------|
| **AI 人工智能** | 1.35 亿 EACO | 10% | AI 推理 API、数据集、模型交易 |
| **WEB3 / 区块链** | 1.35 亿 EACO | 10% | 链上数据 API、钱包服务、节点查询 |
| **RWA（真实世界资产）** | 1.35 亿 EACO | 10% | RWA 数据接口、资产验证 |
| **Stocks（股票）** | 1.35 亿 EACO | 10% | 行情数据 API、交易信号 |
| **Majors（主流货币）** | 1.35 亿 EACO | 10% | 汇率 API、链上 DEX 数据 |
| **Commodities（大宗商品）** | 1.35 亿 EACO | 10% | 黄金/白银价格 API |
| **公益慈善** | 1,350 万 EACO | 1% | 公益项目结算 |
| **十大未来行业** | 各 1,350 万 | 10% | 垂直行业 API 服务 |
| **生态 / 团队 / 储备** | 29% | 29% | 治理投票、生态激励 |

### 5.3 X402 支付市场 | X402 Payment Markets

EACO 在以下市场支持 X402 结算：

| 市场 | 交易对 | 市场地址 |
|------|--------|---------|
| **e-USDT Market** | EACO/USDT | `GeF9cBVQm4A15mcBHxcncXWZrojKsn4zgKEXLDYdmEDi` |
| **e-SOL Market** | EACO/SOL | `7cmAfsKJrvcbMLWoXDXT5uZ2C8udBjzsKEgaxeweKMMU` |
| **e-USDC Market** | EACO/USDC | `Cm6EkxcYNfvxeYDBQ3TGXFqa9NCWvrFKHz4Cfju91dhr` |
| **eCNH-e Market** | EACO/eCNH | `EeECjxqjUALgDEoT6SW2ebN2RTFvN7ci8S8CMaPrLZHw` |
| **WBNB-e Market** | EACO/BNB | `4ELFtiSCM1Ra8yhVFJemnMZPQC2a8yZ3RhSKmckt5Swx` |

---

## 6. Multilingual Support | 多语言支持

EACO X402 支持以下语言，所有支付消息、错误提示、确认页面均提供本地化版本：

### 6.1 支持语言列表 | Supported Languages

| # | 语言 | 语言代码 | 区域 | EACO X402 支持 |
|---|------|---------|------|--------------|
| 1 | **English** | `en` | 联合国官方 | ✅ 完整 |
| 2 | **中文（简体）** | `zh-CN` | 联合国官方 | ✅ 完整 |
| 3 | **中文（繁体）** | `zh-TW` | 联合国官方 | ✅ 完整 |
| 4 | **العربية** (Arabic) | `ar` | 联合国官方 | ✅ 完整 |
| 5 | **Français** (French) | `fr` | 联合国官方 | ✅ 完整 |
| 6 | **Русский** (Russian) | `ru` | 联合国官方 | ✅ 完整 |
| 7 | **Español** (Spanish) | `es` | 联合国官方 | ✅ 完整 |
| 8 | **Bahasa Melayu** (Malay) | `ms` | 东南亚 | ✅ 完整 |
| 9 | **Tiếng Việt** (Vietnamese) | `vi` | 东南亚 | ✅ 完整 |
| 10 | **日本語** (Japanese) | `ja` | 东亚 | ✅ 完整 |
| 11 | **한국어** (Korean) | `ko` | 东亚 | ✅ 完整 |
| 12 | **Português** (Portuguese) | `pt` | 补充 | ✅ 完整 |
| 13 | **Deutsch** (German) | `de` | 补充 | ✅ 完整 |
| 14 | **ภาษาไทย** (Thai) | `th` | 东南亚 | ✅ 完整 |

### 6.2 多语言支付消息模板 | Multilingual Payment Templates

#### 402 支付请求消息 | Payment Required Message

| 语言 | 代码 | 消息模板 |
|------|------|---------|
| **English** | en | `Payment required: {amount} EACO for {service}. Sign transaction to continue.` |
| **中文简体** | zh-CN | `需要支付：{amount} EACO 以使用 {service}。签署交易后继续。` |
| **中文繁体** | zh-TW | `需要支付：{amount} EACO 以使用 {service}。簽署交易後繼續。` |
| **العربية** | ar | `الدفع مطلوب: {amount} EACO مقابل {service}. وقّع المعاملة للمتابعة.` |
| **Français** | fr | `Paiement requis : {amount} EACO pour {service}. Signez la transaction pour continuer.` |
| **Русский** | ru | `Требуется оплата: {amount} EACO за {service}. Подпишите транзакцию для продолжения.` |
| **Español** | es | `Pago requerido: {amount} EACO por {service}. Firme la transacción para continuar.` |
| **Bahasa Melayu** | ms | `Pembayaran diperlukan: {amount} EACO untuk {service}. Tandatangani transaksi untuk teruskan.` |
| **Tiếng Việt** | vi | `Yêu cầu thanh toán: {amount} EACO cho {service}. Ký giao dịch để tiếp tục.` |
| **日本語** | ja | `お支払いが必要です：{service} に {amount} EACO が必要です。取引に署名して続行してください。` |
| **한국어** | ko | `결제 필요: {service} 에 {amount} EACO 가 필요합니다. 거래에 서명하여 계속하세요.` |
| **Português** | pt | `Pagamento necessário: {amount} EACO para {service}. Assine a transação para continuar.` |
| **Deutsch** | de | `Zahlung erforderlich: {amount} EACO für {service}. Unterschreiben Sie die Transaktion, um fortzufahren.` |
| **ภาษาไทย** | th | `ต้องชำระเงิน: {amount} EACO สำหรับ {service} ลงนามธุรกรรมเพื่อดำเนินการต่อ` |

#### 支付成功消息 | Payment Success Message

| 语言 | 代码 | 消息模板 |
|------|------|---------|
| **English** | en | `✅ Payment confirmed. Access granted for {duration}. Tx: {signature}` |
| **中文简体** | zh-CN | `✅ 支付确认。已授予 {duration} 访问权限。交易哈希：{signature}` |
| **中文繁体** | zh-TW | `✅ 支付確認。已授予 {duration} 存取權限。交易哈希：{signature}` |
| **العربية** | ar | `✅ تم تأكيد الدفع. تم منح الوصول لمدة {duration}. المعاملة: {signature}` |
| **Français** | fr | `✅ Paiement confirmé. Accès accordé pour {duration}. Tx: {signature}` |
| **Русский** | ru | `✅ Платёж подтверждён. Доступ предоставлен на {duration}. Tx: {signature}` |
| **Español** | es | `✅ Pago confirmado. Acceso concedido por {duration}. Tx: {signature}` |
| **Bahasa Melayu** | ms | `✅ Pembayaran disahkan. Akses diberikan untuk {duration}. Tx: {signature}` |
| **Tiếng Việt** | vi | `✅ Thanh toán đã được xác nhận. Quyền truy cập được cấp trong {duration}. Tx: {signature}` |
| **日本語** | ja | `✅ 支払い確認完了。{duration} のアクセス権が付与されました。Tx: {signature}` |
| **한국어** | ko | `✅ 결제 확인됨. {duration} 동안 접근 권한이 부여되었습니다. Tx: {signature}` |
| **Português** | pt | `✅ Pagamento confirmado. Acesso concedido por {duration}. Tx: {signature}` |
| **Deutsch** | de | `✅ Zahlung bestätigt. Zugriff gewährt für {duration}. Tx: {signature}` |
| **ภาษาไทย** | th | `✅ ยืนยันการชำระเงินแล้ว ได้รับสิทธิ์เข้าถึง {duration} Tx: {signature}` |

### 6.3 EACO Agent World 区域本地化 | Regional Localization

EACO Agent World 覆盖全球 8 大区域，X402 支付网关支持区域专属本地化：

| 区域 | Agent 名称 | 主要语言 | 本地化重点 |
|------|-----------|---------|----------|
| **全球** | eaco-global | EN/ZH | 通用接口 |
| **欧洲** | eaco-europe-1 | EN/FR/DE/ES/PT/RU | GDPR 合规 API |
| **亚洲** | eaco-asia-1 | ZH/EN/JA/KO | 亚洲市场数据 |
| **美洲** | eaco-americas-1 | EN/ES/PT | 美洲行情 API |
| **MENA** | eaco-mena-1 | AR/EN | 中东市场数据 |
| **东南亚** | eaco-southeast-1 | EN/MS/VI/TH | 东盟经济数据 |
| **南亚** | eaco-south-asia-1 | EN | 南亚市场 |
| **东亚** | eaco-east-asia-1 | ZH/JA/KO | 东亚文化圈 |

---

## 7. Developer Entry | 开发者入口

### 7.1 卖家接入 | Seller Integration (API Providers)

**快速开始：**
1. 克隆 x402-seller-starter：`git clone https://github.com/miraland-labs/x402-seller-starter`
2. 配置 EACO 作为支付代币（修改 `.env` 中的 `TOKEN_MINT`）
3. 部署到 Solana Devnet 测试
4. 迁移到 Mainnet

**EACO 卖家配置示例：**
```python
# EACO X402 Seller Configuration
EACO_CONTRACT = "DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH"
PAYMENT_TOKEN = "EACO"  # Native EACO or USDC

@x402.route("/ai-inference", methods=["POST"])
@x402.payment(amount=100000, token=EACO_CONTRACT, label="AI Inference")
def ai_inference():
    return {"result": "AI inference result", "tokens_used": 1000}
```

### 7.2 买家接入 | Buyer Integration (AI Agents)

**智能体买家（自主程序）：**
```python
# EACO X402 Client
from eaco_x402 import EACOPayer

payer = EACOPayer(
    wallet_path="~/.solana/id.json",
    token_mint="DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH"
)

# 自动处理 402 响应并完成支付
response = payer.request_with_autopay(
    url="https://api.eaco.ai/inference",
    data={"prompt": "Hello EACO"}
)
```

**开发者集成：**
```bash
npm install @eaco/x402-client   # JavaScript/TypeScript
pip install eaco-x402           # Python
cargo add eaco-x402             # Rust
```

### 7.3 发现与监控 | Discovery & Monitoring

- **EACO X402 市场注册表：** https://registry.pr402.org
- **EACO Agent World 发现：** https://eaco-build-world.base44.app
- **链上监控：** [Orb Markets](https://orbmarkets.io/token/DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH)

---

## 8. A Day in the Life of an Agent | 智能体的一天

以下展示一个 EACO AI Agent 的典型一天，全部支付使用 EACO X402：

```
🌅 08:00 UTC — EACO Agent World 每日启动
  ├── 区域合规检查：支付 0.5 EACO → HTTP 402 → 获取合规评分
  │
├── 09:00 UTC — 订阅 EACO AI 数据流
  ├── 调用 /subscribe，支付 10 EACO/月
  ├── 领取 JWT，在订阅期内 Bearer Token 访问
  │   （当日 10,000 次请求，零额外链上交易）
  │
├── 12:00 UTC — 购买 EACO RWA 数据集
  ├── 托管支付 100 EACO → sla-escrow 锁定
  ├── 卖家提交数据集 + Hash 证明
  ├── Oracle 验证通过 → 100 EACO 释放给卖家
  │
├── 15:00 UTC — EACO GameFi API 调用
  ├── 按次支付：0.1 EACO/请求
  ├── 全天累计处理 5,000+ 次请求
  │
└── 22:00 UTC — EACO Agent World 结算
    ├── 当日总支出：约 115.6 EACO
    ├── 收入：来自用户委托的 500 EACO
    └── 净收益：+384.4 EACO（50% 再投资，50% 提现）
```

---

## 9. Security & Risk Management | 安全与风险管理

### 9.1 风险矩阵 | Risk Matrix

| 风险类型 | 概率 | 影响 | 缓解措施 |
|---------|------|------|---------|
| 智能体重复支付 | 中 | 高 | 幂等 Key + 交易签名唯一性验证 |
| EACO 价格波动 | 高 | 中 | USDC 锚定中间层（EACO→USDC 自动兑换） |
| Oracle 验证失败 | 低 | 高 | 多 Oracle 冗余 + 乐观验证 |
| 智能合约漏洞 | 低 | 极高 | 三方审计 + 时间锁发布 |
| 恶意 Agent 拒绝服务 | 中 | 中 | 限流 + IP 白名单 + EACO 押金池 |

### 9.2 防御性架构 | Defensive Architecture

```
┌──────────────────────────────────────────────────┐
│           EACO X402 安全层                        │
│                                                   │
│  1. 支付幂等：每个请求 ID 只扣款一次              │
│  2. 时间锁：托管资金在 Oracle 验证后才释放        │
│  3. 熔断机制：EACO 价格波动 >20% 自动暂停         │
│  4. 限流保护：每 Agent 每分钟最大请求数           │
│  5. 签名验证：多重签名 + HW Wallet 支持           │
└──────────────────────────────────────────────────┘
```

---

## 10. Governance & Upgrades | 治理与升级

### 10.1 协议治理 | Protocol Governance

EACO X402 由 EACO DAO 治理，持有 EACO 的社区成员可对以下事项进行投票：

- ✅ 结算费用率调整（当前：0.3%）
- ✅ 新增支付代币（需 DAO 提案）
- ✅ Oracle 注册与移除
- ✅ 协议升级提案

### 10.2 升级路径 | Upgrade Path

| 阶段 | 时间 | 目标 |
|------|------|------|
| **Phase 1** | 2026 Q3 | EACO X402 MVP，per-call + subscription |
| **Phase 2** | 2026 Q4 | Escrow 模式，Oracle 集成 |
| **Phase 3** | 2027 Q1 | 多链扩展（EVM 兼容链）|
| **Phase 4** | 2027 Q2 | 银河结算层（EACO 10.0 叙事）|

---

## 11. Appendix | 附录

### 11.1 EACO 核心定义 | EACO Core Definition

> **EACO = Earth's Best Coin**
> = Energy（能量）× Attitude（态度）× Cooperation（协作）× Optimization（优化）
> = 正能量 × 积极态度 × 全球协作 × 持续优化
>
> **目标：提升个人认知 · 提升组织效率 · 提升社会文明 · 提升地球可持续发展**
> 与 100 个认知模型结合，构建全球文明升级模型。

### 11.2 合约地址速查 | Contract Address Quick Reference

| 资源 | 地址 |
|------|------|
| **EACO Token (Main)** | `DqfoyZH96RnvZusSp3Cdncjpyp3C74ZmJzGhjmHnDHRH` |
| **EACO AI Market** | `GeF9cBVQm4A15mcBHxcncXWZrojKsn4zgKEXLDYdmEDi` |
| **EACO Web3 Market** | `7cmAfsKJrvcbMLWoXDXT5uZ2C8udBjzsKEgaxeweKMMU` |
| **EACO RWA Market** | `Cm6EkxcYNfvxeYDBQ3TGXFqa9NCWvrFKHz4Cfju91dhr` |
| **EACO Stocks Market** | `EeECjxqjUALgDEoT6SW2ebN2RTFvN7ci8S8CMaPrLZHw` |
| **EACO Charity** | `4ELFtiSCM1Ra8yhVFJemnMZPQC2a8yZ3RhSKmckt5Swx` |
| **EACO Commodities** | `5eEa3mJeRELrry3oEtLSAcsJ8PxLtX7pnTpWLLA9xaeV` |

### 11.3 参考链接 | Reference Links

- **EACO 官网：** https://eaco-build-world.base44.app
- **x402 规范：** https://docs.ipay.sh
- **x402 GitHub：** https://github.com/miraland-labs
- **Solscan：** https://solscan.io
- **Orb Markets：** https://orbmarkets.io

### 11.4 版权声明 | Copyright

© 2026 EACO Foundation. EACO X402 Protocol 是开源协议，采用 MIT 许可证。
EACO X402 Protocol is open-source under MIT License.

---

*EACO — 从地球走向银河的价值基础设施 | Earth's Best Coin — Building Value Infrastructure from Earth to the Galaxy*

**Version: 1.0 | Last Updated: 2026-07-05 | Author: EACO AI Engineer**
