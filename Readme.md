# 精研 APPS ランチャー

株式会社精研の見積もりアプリ群への入口となるランチャーページです。

**URL：** https://seiken-co.github.io/seiken-apps/

---

## 収録アプリ

| アプリ名 | URL |
|---|---|
| 精研見積もり（彩・単頭機・少数頭機） | https://seiken-co.github.io/seiken-estimate/ |
| フリーフォーム見積もり | https://seiken-co.github.io/SEIKEN-FreeForm/ |
| DG.NET SaaS 見積もり | https://seiken-co.github.io/DG.NEt.SaaS/ |

---

## ファイル構成

```
seiken-apps/
├── index.html        # ランチャーページ本体
├── manifest.json     # PWA設定ファイル
├── icom-32.png       # ファビコン
├── icom-180.png      # Apple Touch Icon
├── icom-192.png      # PWAアイコン（小）／ページ内ロゴ
├── icom-512.png      # PWAアイコン（大）
└── README.md         # このファイル
```

---

## 設計方針と経緯

### 1. 事象

ランチャーから各アプリを開いた際、精研見積もりアプリだけEdgeブラウザが一緒に立ち上がる問題が発生。

### 2. 原因

- 精研見積もりのみ `manifest.json` を持っており、PWAとしてインストール済みだった
- `start_url` が固定ファイル（`TEST-7_10.html`）になっており、ランチャーのURLと不一致
- ランチャー側に `target="_blank"` が付いており、OSのPWAルーティングよりブラウザ新規ウィンドウ起動が優先された

結果：精研見積もりだけ「PWAではなくEdgeで開く」挙動になっていた。

### 3. 修正内容

- ランチャーの `target="_blank"` を削除
- URLをディレクトリ直指定に統一
- `start_url` を `"./"` に変更

### 4. 現在の仕様

- ランチャーから各アプリへ通常遷移（ブラウザ内で開く）
- PWAへの個別ルーティングは行わない

### 5. 設計判断

**🎯 方針：ランチャー（精研APPS）を唯一の入口とする。個別アプリのPWAインストールは不要。**

| 理由 | 内容 |
|---|---|
| 構造がシンプル | インストール管理が1本で済む |
| OS依存挙動が消える | ブラウザ差異によるトラブルがなくなる |
| 運用管理が楽 | 将来のバージョン更新が安全 |

### 6. 設計的ポイント（学び）

- PWAは `start_url` と `scope` の一致が絶対条件
- `target="_blank"` はPWA起動と相性が悪い
- 複数PWAを相互起動させるとOS依存の挙動が出る
- 「1つだけPWAにする」方が安定する

### 結論

> ❌ 不具合対応ではなく ✅ アーキテクチャの整理
>
> むしろ一段上の設計に進んだ。

---

*© SEIKEN Co., Ltd.*