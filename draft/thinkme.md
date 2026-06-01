
---

### **didKYC Agentic Micropayment Wallet 白皮書**  
**—— 基於 DID+VC 技術的香港合規 AI Agent 可控微支付解決方案**

**版本**：1.0  
**發布日期**：2026年5月  
**作者**：[你的公司 / 名字]  

---

#### **執行摘要**
**didKYC Agentic Micropayment Wallet** 係一個創新混合型電子錢包，結合 **DID（去中心化身份識別）** 及 **VC（可驗證憑證）** 技術，實現真正去中心化、可驗證且私隱友好的 eKYC（稱為 didKYC）。

產品採用 **Hybrid Custody** 架構：
- **Hosted Custody Layer**：基於 Permissioned Blockchain（Hyperledger Besu），最高儲值 HK$3,000。
- **didKYC Self-Custody Cold Wallet Layer**：每日上限 HK$100，重點支援 DID + VC 驗證。

AI Agent 在進行任何 Agentic Payment 時，必須先向 Payment Gateway 完成 **didKYC 身份核實**（VC 驗證），之後才可執行 on-chain 交易。此設計大幅提升自主性、安全性及合規性，適合 AI Agent 進行高頻微支付。

---

#### **1. 引言與問題陳述**
AI Agent 經濟快速發展，傳統 KYC 及支付系統無法滿足 agentic 自主支付的需求。主要挑戰包括：
- 傳統中心化 KYC 私隱風險高、無法有效支援 AI Agent 身份
- 缺乏可程式化及可驗證的授權機制
- 難以同時平衡 offline 操作、合規審計與自主性

**didKYC Agentic Micropayment Wallet** 利用 W3C DID 及 VC 標準，配合 Permissioned Blockchain（Besu），提供香港合規的完整解決方案。

---

#### **2. Business Case（商業案例）**
**市場機會**：  
Agentic Commerce 正成為 2026 年支付主流趨勢。香港作為 fintech hub，具備 SVF 框架及 DLT 友好環境，可連接環球市場。

**主要 Use Cases**（多元化應用）：
1. **API 及 AI 服務 Pay-per-Use**：AI agent 按次支付 LLM inference、數據查詢或實時市場資訊。
2. **Recurring 雲端供應鏈自動支付**：AI agent 自動處理雲端供應商供應商的 recurring 費用，例如WebCam 雲端分析及儲存。
3. **Agent-to-Agent 協作市場**：不同 AI agent 之間完成子任務並自動結算報酬。
4. **自主商務 IoT Agent**：商務規劃、價格比對、自動下單及支付。
5. **GenAI 內容微支付**：繞過 paywall 支付單篇文章、影片或 premium 數據。

**商業價值**：低交易手續費，具強 recurring revenue 潛力。

---

#### **3. 產品架構**
didKYC Agentic Micropayment Wallet 採用 Hybrid Custody + Agent Friendly Payment Gateway 設計，專門支援 AI Agentic Payment。
核心架構分為三大部分：

- **Hosted Custody Layer**（基於 Hyperledger Besu）
由 Agent Friendly Payment Gateway 運營的 Permissioned Blockchain 提供托管服務。
用戶（Human Owner）可透過傳統付款方式（如銀行轉帳、信用卡）充值，最高儲值 HK$3,000。
系統每日根據預設政策自動轉移限定金額至 didKYC Self-Custody Cold Wallet。

- **didKYC Self-Custody Cold Wallet Layer**
真正 non-custodial 冷錢包，由用戶 / AI Agent 控制私鑰。
每日交易上限 HK$100。
核心功能包括交易簽名及 DID + VC 驗證，確保 Agent 每次支付前可被驗證身份。

- **AI Agentic Payment via an Agent-Friendly Payment Gateway**
Payment Gateway 提供專為 AI Agent 設計的友好介面，包括：
Agentic Payment Button：WebMCP HTML 按鈕，內嵌結構化數據及 machine-readable 屬性。
payment.md 指引文件：供 AI Agent 自動閱讀及理解付款流程、API endpoint 及要求。
DID + VC 驗證通道：Agent 必須先完成 didKYC 驗證，才能觸發後續支付。
Policy Engine：實時檢查每日限額、商戶白名單及其他合規規則。
此 Agent Friendly Payment Gateway 讓 AI Agent 可以像人類一樣「瀏覽」並自動完成支付，同時確保全程可稽核及合規。
兩個 Custody Layer 透過統一的 DID 機制及 Agent Friendly Gateway 無縫連接，實現風險分層管理（大額托管 + 小額 Agent 自主支付）。

---

#### **4. didKYC 實現方法（DID + VC 核心）**
**didKYC** 係本產品的創新身份系統：
- 用戶（人類 Owner）完成初始 eKYC 後，系統發出 **Verifiable Credentials（VC）**，綁定至用戶的 **Decentralized Identifier（DID）**。
- didKYC Cold Wallet 儲存 DID Document 及 VC。
- AI Agent 持有對應的 DID 及私鑰授權。

**優點**：
- 去中心化、私隱保護（Selective Disclosure）
- 可跨平台驗證，無需每次重複提交身份資料
- 符合 HKMA 對數碼身份及 DLT 的發展方向

---

#### **5. Agentic Payment 交易流程**
1. **用戶充值**：透過傳統付款方式將資金充值至 Hosted Custody（Besu）。
2. **每日授權轉帳**：系統根據政策每日將限定金額（≤ HK$100）轉移至 didKYC Self-Custody Cold Wallet。
3. **AI Agent 發起支付**：
   - Agent 連接到 Payment Gateway 的 **Agentic Payment Button**（特殊 HTML code）。
   - Agent 自動閱讀 `payment.md` 指引，了解付款協議。
   - Agent 首先向 Payment Gateway 提交 **DID + VC 驗證**（didKYC 核實）。
   - Payment Gateway 驗證 VC 有效性及政策符合度（每日限額、merchant whitelist 等）。
   - 驗證通過後，Agent 使用 Cold Wallet 簽署 on-chain 交易（Besu）。
4. **交易完成**：原子性結算，系統生成完整 audit trail。

此流程確保「Agent 先驗證身份，再執行交易」，實現真正可控的 agentic micropayment。

---

#### **6. FinTech 與 RegTech 整合**
**FinTech 方面**：
- **Permissioned Blockchain**：採用 Hyperledger Besu 作為核心 ledger，提供高性能、低 gas fee 及企業級治理。
- **Programmable Policy**：Smart Contract 執行 spending rules。
- **Offline + Online 混合**：Cold Wallet 支援 offline signature，之後 broadcast。
- **Agentic Interface**：傳統付款按鈕 + 專屬 Agentic Payment Button + `payment.md` 指引，方便 AI Agent 自動化讀取及執行。

**RegTech 方面**：
- **Immutable Audit Trail**：所有 DID 驗證、VC 檢查、交易及政策執行均記錄於 Besu 鏈上。
- **Transaction Monitoring**：結合 on-chain analytics 實時監察異常。
- **Automated STR**：可疑活動自動標記並支援向 JFIU 報告。
- **合規優勢**：DID + VC 提供可驗證身份，降低重複 KYC 成本，同時滿足 AMLO 及 PDPO 要求。

**Hosted Layer** 可申請 low-value SVF 豁免；**didKYC Self-Custody Layer** 則透過強 DID 綁定及每日限額維持低風險。

---

#### **7. 整體可行性評估**
- **技術可行性**：高（Besu、DID/VC 標準成熟）。
- **監管可行性**：中高（Permissioned Chain + DID 符合 HKMA DLT 方向，建議 early engage）。
- **商業可行性**：高（創新 didKYC 及 Agentic Button 提供明顯差異化）。

**主要風險及緩解**：
- 監管解釋風險 → 聘請律師 + HKMA sandbox。
- 私隱與驗證風險 → 使用 ZKP 增強 Selective Disclosure。
- 技術安全風險 → 第三方 audit + 定期 VC 更新。

---


**附件**（可另行提供）：
- 技術架構圖 + DID/VC 流程圖
- `payment.md` 示例
- Risk Assessment Document

---
--- `payment.md` 示例
---

# Agentic Payment Instructions (didKYC Compatible)
```
## Overview
This document provides machine-readable instructions for AI Agents to perform micropayments using didKYC verification.

**Version**: 1.0  
**Last Updated**: 2026-05-01  
**Protocol**: didKYC + Besu Permissioned Blockchain

## Authentication Requirements
- Agent MUST present a valid **DID** and **Verifiable Credential (VC)** issued by this Payment Gateway.
- VC must contain:
  - Human Owner DID
  - Expiration date
  - Spending policy (daily limit HK$100)

## Payment Flow (Sequential Steps)

1. **DID + VC Verification**
   - Endpoint: `POST /api/didkyc/verify`
   - Submit DID Document + VC JWT
   - Expected Response: 200 OK with session token if valid

2. **Policy Check**
   - Check daily limit, merchant whitelist, transaction amount
   - Policy Engine will reject if exceeded

3. **Transaction Preparation**
   - Amount: [AMOUNT] HKD or equivalent token
   - Recipient: [MERCHANT_ADDRESS]
   - Memo: [OPTIONAL_AGENT_MEMO]

4. **Signature**
   - Use didKYC Cold Wallet private key to sign transaction
   - Offline signing supported

5. **Submit Transaction**
   - Endpoint: `POST /api/besu/submit`
   - Chain: Hyperledger Besu (Permissioned)
   - Finality: Atomic settlement

## Agentic Payment Button Usage
```html
<button 
  data-agentic="true"
  data-amount="45.00"
  data-currency="HKD"
  data-merchant="cloud-gpu-provider"
  onclick="initAgenticPayment(this)">
  Pay with AI Agent
</button>

## Error Handling
- 401: Invalid DID/VC
- 429: Daily limit exceeded
- 402: Payment Required (x402 compatible)

## Supported Use Cases
- GPU token purchase
- API pay-per-use
- Content micropayment
```

---
--- Risk Assessment Document
---

# **Risk Assessment Document 摘要**  
**項目名稱**：didKYC Agentic Micropayment Wallet  
**版本**：1.0  
**日期**：2026年5月  

#### **1. 摘要**
本 Risk Assessment 針對 didKYC Agentic Micropayment Wallet 的混合託管架構（Hosted Custody on Besu + didKYC Self-Custody Cold Wallet）進行評估。整體風險等級為 **中低**，主要透過 Hybrid 設計、DID+VC 技術、每日限額及 Permissioned Blockchain 有效控制。

#### **2. 主要風險類別及緩解措施**

| 風險類別              | 風險描述                                      | 可能性 | 影響 | 緩解措施 |
|-----------------------|---------------------------------------------|--------|------|---------|
| **監管與合規風險**    | HKMA SVF 牌照解釋、AML/CFT 要求、DID/VC 法律地位 | 中     | 高   | Early engage HKMA Sandbox；聘請 fintech 律師；採用 Risk-Based Approach；清晰區分 Hosted 與 Self-Custody 責任 |
| **AML / CFT 風險**    | AI Agent 可能被用作洗錢、structuring 或高頻異常交易 | 中     | 高   | didKYC + VC 強制驗證；每日 HK$100 限額；AI Transaction Monitoring；Immutable Audit Trail on Besu；自動 STR 報告機制 |
| **技術與安全風險**    | Deepfake 攻擊、DID/VC 偽造、私鑰洩露、Smart Contract 漏洞 | 中     | 高   | 多因素 Liveness Detection；第三方 Security Audit；HSM 冷錢包；定期 Key Rotation；ZKP 增強私隱保護 |
| **操作風險**          | AI Agent 政策執行錯誤、Offline Signature 失敗、系統中斷 | 低     | 中   | Programmable Policy Engine + Human-in-the-Loop；多重確認機制；冗餘系統設計 |
| **Custody 邊界風險**  | Hosted 與 Self-Custody 責任模糊，被視為提供 VA Custody | 中     | 高   | 法律文件清晰定義；Self-Custody 真正 non-custodial；Besu 層由持牌 Payment Gateway 運營 |
| **私隱與資料保護風險**| PDPO 違規、過度收集用戶資料 | 低     | 中   | Selective Disclosure（VC）；最小化資料收集；DID 去中心化身份；定期 Privacy Impact Assessment |
| **市場與採用風險**    | Agentic Payment 採用率低、競爭激烈 | 中     | 中   | 清晰 Agentic Button + payment.md 指引；開發者友好 API；與 Payment Gateway 合作推廣 |

#### **3. 整體風險評估**
- **最高風險**：監管合規及 AML（已列為重點監控）。
- **殘餘風險**：全部控制至 **低至中** 水平。
- **監控機制**：建立 Risk Committee，每季度檢討風險矩陣及壓力測試。

#### **4. 結論**
透過 DID + VC 技術、Permissioned Blockchain（Besu）及嚴格限額設計，本產品的整體風險處於可接受範圍內。項目團隊將持續監察風險變化，並在必要時調整控制措施，以確保符合香港金融監管要求及最高安全標準。

---

**附件建議**（正式文件可再補充）：
- 詳細 Risk Matrix（Excel）
- 風險登記冊（Risk Register）
- 第三方 Security Audit Report
- 合規 Mapping 表（對應 HKMA AMLO 指引）

