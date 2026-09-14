# MVP 接力操作基線（已實測 · 鎖定 2026-09-14）

**狀態：** 規則已鎖定。驗收 **A／B 尚未完成** → MVP **尚未**標為可用。  
**不擴充新架構；不建常駐執行 Bot；不要求修通已有替代的原生接點。**

Skill：[vibe-coding-relay-mvp]（協調者本機 skill） · Issue 錨點：[#3](https://github.com/kaoru0429/ai-wait-compare/issues/3)

---

## 固定規則（10）

1. **唯一入口**：幕僚長助手（Grok Bot）負責需求釐清、派工、追蹤、收件與回報；使用者不搬 Prompt、不搬審核結果。
2. **預設卡司**：Jules Pro 施工；GPT Codex 獨立審核；Claude Code **client** 作備援審查。
3. **Jules 開不出 PR**：協調者把 **同一份** Jules `changeSet` 轉寫成 GitHub PR，保留來源；**禁止**另寫一份修復冒充 Jules 成果。官方 API 無 Publish 端點（僅 create／get／list／approvePlan／sendMessage）；`AUTO_CREATE_PR` 能開就用。
4. **原 Session 無法續修**：可開新 Session 接同一 Issue，留下 Session 對應，避免舊工作重複交付。
5. **複核**：對準**最終待合併 commit**；相關驗收通過後，依既有授權合併與發布。
6. **修復輪次**：同一驗收條件最多正式修復 **兩輪**。使用者新增需求另記變更。接入失敗、重新派送 **不**冒算成程式修復輪次。
7. **GitHub 必存欄位**：需求與驗收、施工平台、Session、PR、受審版本、審核結果、目前負責者、最後完成階段與下一步。
8. **續接**：既有授權有效時直接續接；只有必要登入／授權、重大取捨或修復達上限才找使用者。
9. **結案固定回報**（三分＋要否處理）：
   - 已通過＋證據  
   - **未通但已有替代**（未通項 → 替代項 → 替代是否實測）  
   - 未通且無替代  
   - 需要使用者處理什麼  
10. **暫不**建立常駐執行 Bot；也**不要求**修通已有替代的原生接點。

---

## 驗收清單（完成前不標 MVP 可用）

| ID | 項目 | 狀態 |
|---|---|---|
| **A** | 正常使用：匯出／匯入（Issue #7／PR #8） | **功能已合併**；待使用者預覽驗收；Codex 剩餘 P2 輪次用盡 |
| **B** | 接手恢復：最小一次性測試；接手者依 GitHub 紀錄找回工作、確認下一步且不重複派工；先確認原負責者已停止派工 | **尚未完成** |

A、B 皆通過後，於本檔與 `PROGRESS.md` 將 MVP 標為 **可用**。

---

## 本輪已實測證據（規則來源，非 A／B）

### 已通過
- Codex／Claude client 審；Jules 補丁；changeSet→PR #5→合併 `e180c06`；Pages

### 未通但已有替代（已實測）
- Jules 自開 PR → GitHub API 轉寫 changeSet  
- `@claude` GitHub → Claude Code client  
- 原 Session 續修 → 新 Session  

### 未通且無替代
無（就本輪交付而言）

### 需要使用者處理
規則保存階段：無。驗收 A 待使用者下一個小需求；驗收 B 待安排接手測試。

參考：https://developers.google.com/jules/api
