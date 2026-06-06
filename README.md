# FireSign 運用マニュアル

**バージョン:** 1.0  
**作成日:** 2026-06-06  
**対象:** FireSign 管理者・運用スタッフ

---

## システム構成

```
管理画面 (firesign_admin.html)
    ↓ config.json を編集・ダウンロード
Google Drive (firesign フォルダ)
    ↑ config.json を読み込む（60秒ごと）
プレーヤー (index.html) ← FireTV Silk ブラウザで開く
```

## URL 一覧

| 用途 | URL |
|---|---|
| プレーヤー（FireTV用） | https://take4-minami.github.io/firesign/ |
| 管理画面（設定変更用） | https://take4-minami.github.io/firesign/firesign_admin.html |
| GitHubリポジトリ | https://github.com/Take4-Minami/firesign |
| Google Drive firesignフォルダ | https://drive.google.com/drive/folders/1PYLYwXCaEsP1v0GNHAk6DLVtWtLjk5HL |

---

## 日常操作：テロップ・コンテンツを変更する

### 手順

**① 管理画面を開く**

ブラウザで以下を開く：
```
https://take4-minami.github.io/firesign/firesign_admin.html
```

**② 現在の設定を読み込む**

右上の「☁ Driveから読込」ボタンをクリック  
→ Google Drive の現在の config.json が画面に反映される

**③ 内容を編集する**

- **テロップ文章**：テキストエリアを直接書き換える
- **テロップON/OFF**：トグルボタンで切り替え
- **スクロール速度**：数値を変更（大きいほど速い）
- **プレイリスト**：
  - 追加：下部フォームにURLを入力して「追加する」
  - 削除：各アイテムの「✕」ボタン
  - 並び替え：左端の「↕」をドラッグ

**④ config.json を保存する**

「↓ config.json を保存」ボタンをクリック  
→ `config.json` がダウンロードフォルダに保存される

**⑤ Google Drive に上書きアップロード**

1. Google Drive を開く：https://drive.google.com
2. 「マイドライブ」→「firesign」フォルダを開く
3. ダウンロードした `config.json` をフォルダにドラッグ＆ドロップ
4. 「既存のファイルを置き換えますか？」→「置き換える」をクリック

**⑥ プレーヤーへの反映を確認**

- プレーヤーは **60秒ごと** に自動で設定を再読み込みする
- すぐに反映させたい場合はプレーヤーのページを再読み込みする（FireTV: ホームボタン→Silkブラウザを再起動）

---

## コンテンツ（画像・動画）をGoogle Driveから表示する

### Google Drive のファイルを直リンクに変換する手順

**① Google Drive にファイルをアップロード**

1. Google Drive の `firesign` フォルダを開く
2. 画像（jpg/png）または動画（mp4）をドラッグ＆ドロップ

**② ファイルを「リンクを知っている全員」に共有**

1. ファイルを右クリック →「共有」
2. 「一般的なアクセス」を「リンクを知っている全員」に変更
3. 「完了」をクリック

**③ ファイルIDを取得**

共有リンクをコピーすると以下の形式になる：
```
https://drive.google.com/file/d/【FILE_ID】/view?usp=sharing
```
`【FILE_ID】` の部分（長い英数字）をコピーする

**④ 直リンクURLを作成**

```
https://drive.google.com/uc?export=download&id=【FILE_ID】
```

このURLを管理画面のプレイリスト追加フォームに入力する

---

## FireTV Stick の初期設定（新しいTVに追加する場合）

1. FireTV Stick をテレビのHDMIに接続、電源を入れる
2. Amazonアカウントでログイン
3. ホーム画面で「アプリ」→「ブラウザ」→「Silk ブラウザ」をインストール
4. Silk ブラウザを起動
5. アドレスバーに以下を入力：
   ```
   take4-minami.github.io/firesign/
   ```
6. 画面をクリックするとフルスクリーンになる
7. フルスクリーンのまま放置するとサイネージとして動作する

---

## トラブルシューティング

| 症状 | 原因 | 対処 |
|---|---|---|
| 画面が真っ黒のまま | ネットワーク未接続 | FireTVのWi-Fi設定を確認 |
| テロップが古いまま | Drive同期待ち | 60秒待つかブラウザを再読み込み |
| 「画像の読み込みに失敗」 | URLが間違っている | 管理画面でURLを確認・修正 |
| 管理画面でDrive読込失敗 | APIキーの制限 | Google Cloud ConsoleでAPIキーを確認 |
| 動画が再生されない | FireTVの音声制限 | 動画はmuteにして再アップロード |

---

## ファイル構成

```
GitHub (take4-minami/firesign)
├── index.html          ← プレーヤー本体
└── firesign_admin.html ← 管理画面

Google Drive (firesign フォルダ)
├── config.json         ← 設定ファイル（管理画面で編集）
├── *.jpg / *.png       ← 画像コンテンツ
└── *.mp4               ← 動画コンテンツ
```

---

## config.json の構造（参考）

```json
{
  "screenIndex": 0,
  "telop": {
    "enabled": true,
    "text": "テロップ文章",
    "speed": 80,
    "position": "bottom",
    "fontSize": 26,
    "bgColor": "rgba(0,0,0,0.75)",
    "textColor": "#ffffff"
  },
  "playlist": [
    {
      "type": "image",
      "url": "https://...",
      "duration": 5
    },
    {
      "type": "video",
      "url": "https://..."
    }
  ],
  "loop": true,
  "errorRetry": 10,
  "videoObjectFit": "contain"
}
```

---

*FireSign — オープンソース デジタルサイネージシステム*
