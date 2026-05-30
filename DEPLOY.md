# TIME ATTACKER — GitHub Pages デプロイ手順

## デプロイするファイル一覧

```
リポジトリルート/
├── index.html      ← メイン HTML (変更なし)
├── app.js          ← ロジック (変更なし)
├── styles.css      ← スタイル (変更なし)
├── manifest.json   ← PWA マニフェスト (変更なし)
├── sw.js           ← Service Worker (変更なし)
├── _config.yml     ← Jekyll バイパス (★追加)
├── icon-192.png    ← PWA アイコン (別途用意)
└── icon-512.png    ← PWA アイコン (別途用意)
```

> `main.py` と `_pta_index.html` は **不要**。アップロードしないこと。

---

## デプロイ手順

### 1. リポジトリ作成 (初回のみ)

```bash
git init
git remote add origin https://github.com/<username>/<repo>.git
```

### 2. ファイルをプッシュ

```bash
git add index.html app.js styles.css manifest.json sw.js _config.yml icon-192.png icon-512.png
git commit -m "TIME ATTACKER initial deploy"
git push -u origin main
```

### 3. GitHub Pages を有効化

リポジトリ → Settings → Pages → Source: `Deploy from a branch` → Branch: `main` / `/ (root)`

### 4. アクセス URL

```
https://<username>.github.io/<repo>/
```

---

## 動作確認チェックリスト

- [ ] スプラッシュ → 注意事項画面が表示される
- [ ] コース作成・マップ描画ができる
- [ ] GPS 計測が開始できる (ブラウザの位置情報許可が必要)
- [ ] G センサーが反応する (iOS Safari: 「センサー許可」ボタンをタップ)
- [ ] CSV 出力ボタンでファイルがダウンロードされる
- [ ] PWA としてホーム画面に追加できる

---

## iOS Safari 固有の注意

### GPS 精度
- Safari では `watchPosition` の精度がアプリ版より低い場合がある
- 設定画面 → コース詳細 の「GPS精度フィルタ」を `30m` → `50m` に緩めると安定する

### センサー許可 (G ボール)
- iOS 13以降、`DeviceMotionEvent.requestPermission()` が必要
- 走行画面下部の **「センサー許可」ボタン** をタップすること
- HTTPS でないと許可ダイアログが出ない (GitHub Pages は HTTPS ✅)

### OBD2 BLE
- iOS Safari は Web Bluetooth 非対応のため OBD2 は無効化
- 設定画面の OBD 項目はグレーアウトされ、スキャンボタンは「Web Bluetooth 非対応」トーストが表示される
- GPS + G センサーのみで計測可能

---

## CSV ダウンロード動作

| 画面 | ボタン | ファイル名形式 |
|------|--------|---------------|
| 走行画面 | CSV 出力 | `mirage_コース名_YYYY-MM-DDTHH-MM-SS.csv` |
| 履歴詳細 | CSV エクスポート | `timeattacker_コース名_YYYY-MM-DDTHH-MM-SS.csv` |

ブラウザのダウンロードフォルダに保存される。
iOS Safari では「ファイル」アプリへの保存ダイアログが表示される。
