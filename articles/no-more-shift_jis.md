---
title: "Shift_JIS撲滅委員会"
emoji: "🧟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Shift_JIS]
published: false
---

ここに**Shift_JIS撲滅委員会**を発足します。

合言葉は，

**NO MORE Shift_JIS**

です。

## Shift_JIS撲滅委員会の掟

- 過去の世界を支えたShift_JISという技術に敬意を表する
- レガシーシステムに潜むShift_JISを極力廃止し，UTF-8に移行する
- 新規開発でShift_JISに依存したシステムをこれ以上増やさない

## 実践内容

### 共通

- Shift_JISにせよ，UTF-8にせよ，IDEやコンパイラやソースコードなどではエンコードを意識的に明示する
- 自システムをUTF-8にする場合もShift_JISに依存する外部システムに配慮する
- ソースコードはUTF-8で書く
- もしもUTF-8より優れたスタンダードとなる技術が現れたら乗り換えるべき検討する

### Windows

- サポートされている新しいWindowsを使う
- テキストエディタのデフォルト設定をUTF-8にする
- PowerShellを使う(cmdやWindows PowerShellは使わない)
- メーラーなど，使っているツールにShift_JIS設定があったらUTF-8にする(周りに迷惑をかけない範囲で)

## Shift_JISって何？UTF-8って何？って人

Shift_JIS, Unicode, UTF-8, EUC-JPなどのキーワードでググったりChatGPTに質問したりしてみましょう。

## どうしてUTF-8にするの？

細かい説明は省略しますが，地球上の全人類がUTF-8を使うことで，
今後作られるコンテンツで文字化けが無くなり，
業務システムで可愛い絵文字を自由に使えるようになり，
役所で働く方々がIVSの御力で外字登録作業から解放されます。

## あとがき

炎上しないか冷や冷やしています。
