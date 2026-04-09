# film-contract-kit

拍片仔的合約工具包 — 影像製作合約包產生器與審計工具。

專為獨立影像工作者 / 小型製作團隊設計，搭配 [Claude Code](https://claude.ai/code) 使用，透過 AI 互動式問答自動產生整套合約文件，並提供邏輯審計與一致性檢查。

---

## 這是什麼？

接案拍片最煩的不是拍攝，是那堆合約文件。每次開新案要準備：

- 合約書（影片版 or 平面版）
- 製作時程表
- 設備清單
- 拍攝通告單
- 鏡頭配置與分鏡表

這些文件之間高度關聯 — 日期、金額、人員、設備必須一致，少填一個欄位就是隱患。

**film-contract-kit** 把這套流程變成一個 Claude Code Skill：

1. 問答式收集資料（不用一次填 30 個欄位）
2. 自動從模板產生整包文件
3. 三層審計檢查（邏輯 → 一致性 → 完整性）
4. 補漏提醒（哪些沒填、哪些 PDF 沒轉）

---

## 模板清單

| # | 文件 | 檔案 | 說明 |
|:-:|------|------|------|
| 1 | 影像合約書（影片） | `contract-video.md` | 完整法律合約 + 附件報價單 |
| 2 | 攝影合約書（平面） | `contract-photo.md` | 平面攝影專用合約 |
| 3 | 製作時程表 | `schedule.md` | 從簽約到結案的時間節點 |
| 4 | 設備清單 | `equipment-list.md` | 機身/鏡頭/燈光/收音/配件 |
| 5 | 拍攝通告單 | `call-sheet.md` | 當日 rundown + 工作人員 |
| 6 | 鏡頭配置與分鏡表 | `lens-storyboard.md` | 鏡頭分配 + 場景分鏡 |

所有模板使用 `{{placeholder}}` 語法，支援直接編輯或透過 Skill 自動填充。

---

## 四種操作模式

### [A] 新案建立
分組問答收集甲乙方資料、專案規格、財務、時程、團隊 → 自動產生整包文件 → 跑一次審計。

### [B] 單檔產生
只需要補一份通告單？選編號、填資料、產出，順便交叉核對同案已有文件。

### [C] 合約審計
三層檢查：
- **Layer 1** 合約邏輯 — 金額加總、日期順序、條款完整性
- **Layer 2** 跨文件一致性 — 10+ 欄位交叉比對，不一致立即標記
- **Layer 3** 時間線對齊 — 所有文件的日期匯成統一時間軸

### [D] 補漏檢查
掃描 placeholder 殘留、空白表格、PDF 缺漏，列出待辦清單。

---

## 安裝與使用

### 前置需求
- [Claude Code](https://claude.ai/code)（CLI / Desktop / Web）

### 安裝

```bash
claude skill add --from https://github.com/ourladypeace2011-commits/film-contract-kit
```

或手動複製：

```bash
# 複製到你的專案 skills 目錄
git clone https://github.com/ourladypeace2011-commits/film-contract-kit.git
cp -r film-contract-kit/SKILL.md your-project/skills/film-contract-kit/
cp -r film-contract-kit/templates/ your-project/templates/
```

### 使用

在 Claude Code 中輸入：

```
/film-contract-kit
```

或直接說：

```
幫我開一個新案的合約
合約審計 恬馨幼兒園
```

---

## 檔案結構

```
film-contract-kit/
├── README.md
├── SKILL.md              ← Claude Code Skill 定義
├── templates/            ← 合約模板（{{placeholder}} 語法）
│   ├── contract-video.md
│   ├── contract-photo.md
│   ├── schedule.md
│   ├── equipment-list.md
│   ├── call-sheet.md
│   └── lens-storyboard.md
└── examples/             ← 範例輸出（匿名化）
```

---

## 文件間依賴關係

```
合約書 (master)
 ├─→ 製作時程表（日期、金額、甲乙方）
 ├─→ 報價單（嵌入合約附件二）
 │
 拍攝日確定後 ──→
 ├─→ 設備清單（日期、專案名）
 ├─→ 拍攝通告單（日期、地點、人員、rundown）
 └─→ 鏡頭分鏡表（人員、鏡頭、場景）
      │
      └─ 設備清單 ←→ 鏡頭分鏡表（鏡頭互相引用）
         通告單 ←→ 鏡頭分鏡表（人員、場景互相引用）
```

合約書是 master document — 修改金額或日期時，下游文件需同步。Skill 的審計模式會自動偵測不一致。

---

## 自訂模板

模板使用 Markdown + `{{placeholder}}` 語法，可自由修改：

- 新增條款：直接編輯 `contract-video.md` 或 `contract-photo.md`
- 新增文件類型：在 `templates/` 新增 `.md`，並更新 `SKILL.md` 的 Template Registry
- 調整問答流程：修改 `SKILL.md` 中的欄位分組

---

## 注意事項

- 本工具產出的合約為**範本**，請依據實際需求調整並諮詢法律專業人士
- 合約條款以**中華民國法律**為準據法，管轄法院預設為臺灣臺北地方法院
- 模板中的著作權條款為常見架構，重大案件建議另行擬定
- 個資欄位（統編、電話、地址）不會自動進入 git commit message

---

## 授權

MIT License

---

> 拍片仔，把時間花在創作上，不要花在填表上。
