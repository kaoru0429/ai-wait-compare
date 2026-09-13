# 自動接力操作備忘（v1 — 2026-09-14）

小白只提需求。協調者（Grok Bot）負責授權後的全自動接力。不開專屬執行 Bot，直到本流水線多跑幾次穩定。

## 正式流水線

1. **需求入口**：本 repo Issue／聊天對協調者說目標。
2. **施工（預設 Jules Pro）**
   - REST 建 Session：`prompt` 以 `//` 開頭；`sourceContext.source = sources/github/kaoru0429/ai-wait-compare`
   - 一律帶 `automationMode: "AUTO_CREATE_PR"`、`requirePlanApproval: false`（能開 PR 就開）
   - 官方 Jules API **沒有** Publish／CreatePR 獨立端點（僅 create / get / list / approvePlan / sendMessage）
3. **發布（雙路徑）**
   - **A（優先）**：輪詢 Session `outputs[].pullRequest` → 用 Jules 開出的 PR
   - **B（取代方案，已實測）**：有 `changeSet`／gitPatch 但無 PR → 協調者用 **GitHub Contents／Pulls API** 把 **同一份 Jules 補丁** 開 PR／合併  
     - 施工者＝Jules；發布者＝Grok Bot API 轉寫  
     - **禁止**為了掩蓋 Jules 開 PR 失敗而自行發明另一套修復
4. **第二審（獨立 AI）**
   - **預設**：GitHub `@codex review`（已通）
   - **備援**：Claude Code **client**（box 登入一次後由協調者 `-p` 代送）— 不依賴 `@claude` GitHub App
   - **禁止**用協調者自審冒充第二 AI
5. **複核 → 合併 → Pages**；進度寫 Issue + `PROGRESS.md` / `BUILD_LOG.md`

## 已知未通（不要當主路徑）

| 接點 | 狀態 | 取代 |
|---|---|---|
| Jules `AUTO_CREATE_PR`／App Publish | 不穩；App「Ready for review」需人按，API 代按不了 | 路徑 B：GitHub API 轉寫 changeSet |
| Jules 原 Session 續修（COMPLETED 後 sendMessage） | 半通／常無新產出 | 開新 Session 修同一 Issue |
| Claude GitHub `@claude review` | 未通 | Claude Code client 代審 |

## 證據錨點（本輪接力）

- Issue：https://github.com/kaoru0429/ai-wait-compare/issues/3
- Codex 初審：https://github.com/kaoru0429/ai-wait-compare/pull/4#pullrequestreview-5192475274
- Jules 補丁 Session：https://jules.google.com/session/13615700408848045305
- 路徑 B 落地 PR：https://github.com/kaoru0429/ai-wait-compare/pull/5 （已合併 `e180c06`）
- 預覽：https://kaoru0429.github.io/ai-wait-compare/

## 參考

- Jules API：https://developers.google.com/jules/api
- Sessions reference：https://developers.google.com/jules/api/reference/rest/v1alpha/sessions
