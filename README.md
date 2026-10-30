# Shhelf 字卡格式

[Shhelf](https://apps.apple.com/tw/app/id6820650706) 的字卡可以用這個格式分享給別人，或請你自己的 AI（ChatGPT、Claude、Gemini…）從講義裡整理單字，再匯入 Shhelf。

哪些詞要、哪些不要，你自己跟 AI 講清楚，AI 照格式整理好，你只要貼進 Shhelf 就行。

## 怎麼用

1. 把下面「給 AI 的說明」整段複製給你的 AI，在最後寫上你的要求，再附上講義（PDF、文字、截圖都可以）。
   - Shhelf 裡也有同一段：**字卡 → 右上角「匯入字卡」→「複製給 AI 的說明」**。
2. AI 回完以後，把它的回答整段複製起來。前後多寫的說明不用刪，Shhelf 只會讀字卡的部分。
3. 打開 Shhelf：**字卡 → 匯入字卡 →「貼上」**，確認後會開一個新牌組。

如果 AI 給的是檔案，也可以存成 `.json` 或 `.shhelf`，用「從檔案匯入」選它。

## 給 AI 的說明

````text
請把我給你的資料整理成 Shhelf 字卡,只輸出下面這種 JSON,不要加其他文字:

{
  "format": "shhelf-deck",
  "version": 1,
  "name": "牌組名稱",
  "cards": [
    { "front": "mitral valve", "back": "二尖瓣", "note": "左心房和左心室之間" },
    { "front": "chordae tendineae", "back": "腱索" }
  ]
}

規則:
- front:正面(通常是英文名詞),照原文拼法,不要改大小寫或縮寫。
- back:背面(通常是中文翻譯)。
- note:補充說明,可以省略。
- 同一個 front 只出現一次。
- 雙引號、反斜線要照 JSON 規則跳脫。

我的要求(哪些要、哪些不要):
````

要求可以這樣寫：

- 「只要解剖構造，不要藥名」
- 「只抓投影片上粗體的詞」
- 「back 寫中文加一句定義」
- 「縮寫的話，front 寫縮寫，note 寫全名」

## 分享給別人的 Shhelf

打開牌組，按右上角 **⋯ →「分享牌組（給別人的 Shhelf）」**，用 AirDrop、LINE 或 Mail 傳出去。對方點開 `.shhelf` 檔就能匯入。

分享出去的只有正面、背面、補充，**不包含你的複習進度**，對方會從新卡開始。

## 格式說明

`.shhelf` 檔就是 UTF-8 的 JSON：

| 欄位 | 必填 | 說明 |
|---|---|---|
| `format` | 建議 | 固定寫 `"shhelf-deck"` |
| `version` | 建議 | 目前是 `1` |
| `name` | 否 | 牌組名稱；沒寫就用檔名，貼上的叫「匯入的字卡」 |
| `cards` | 是 | 字卡清單 |
| `cards[].front` | 是 | 正面；空白的會跳過 |
| `cards[].back` | 否 | 背面 |
| `cards[].note` | 否 | 補充 |

匯入時的處理：

- 每張卡前後的空白會去掉。
- `front` **完全一樣**的只留第一張。大小寫不同就算不同的詞（例如 `ACE` 和 `ace`），不做模糊比對。
- 一律開一個新牌組，不會改到你現有的字卡和複習紀錄。
- 一次最多 10,000 張。

範例：[`example.shhelf`](example.shhelf)

## 從 Anki 搬過來

Anki 選 **匯出 → 筆記為純文字（Notes in Plain Text）**，在 Shhelf 用「從檔案匯入」選那個 `.txt`。

- 每行一張卡，用 Tab 分開：第一欄是正面，第二欄是背面，第三欄以後當補充。
- `#` 開頭的設定行會跳過。

---

English summary: a `.shhelf` file is UTF-8 JSON, `{"format":"shhelf-deck","version":1,"name":"…","cards":[{"front":"…","back":"…","note":"…"}]}`. Only `cards[].front` is required. Shhelf also accepts the JSON pasted inside an AI reply (surrounding text is ignored), and Anki tab-separated plain-text exports.
