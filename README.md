# FireSign 運用マニュアル

**バージョン:** 2.0
**更新日:** 2026-07-08
**対象:** FireSign 管理者・運用スタッフ

---

## システム構成

```
管理画面 (firesign_admin.html)
    ↓ config.json を編集・ダウンロード
Google Drive (firesign フォルダ)
    ↑ config.json ＋ 画像ファイルを読み込む（60秒ごと）
プレーヤー (index.html) ← FireTV Silk ブラウザで開く
```

v2.0 でマルチゾーンレイアウトに対応。画面を最大4分割し、ゾーンごとに独立したコンテンツ（画像は自動スキャン、動画はYouTube限定公開）を再生できます。

## URL 一覧

| 用途 | URL |
|---|---|
| プレーヤー（FireTV用） | https://take4-minami.github.io/firesign/ |
| 管理画面（設定変更用） | https://take4-minami.github.io/firesign/firesign_admin.html |
| GitHubリポジトリ | https://github.com/Take4-Minami/firesign |
| Google Drive firesignフォルダ | https://drive.google.com/drive/folders/1PYLYwXCaEsP1v0GNHAk6DLVtWtLjk5HL |

---

## 画面レイアウト（7パターン）

管理画面の「画面レイアウト」で選択します。レイアウトごとにゾーン（表示領域）が決まっています。

| レイアウトID | 内容 | ゾーンID |
|---|---|---|
| `full` | 全画面 | main |
| `main-bottom` | メイン(上80%) + 下帯(20%) | main, bottom |
| `main-top` | 上帯(20%) + メイン(下80%) | top, main |
| `split-lr` | 左右2分割 | left, right |
| `split-tb` | 上下2分割 | top, bottom |
| `main-sidebar` | メイン(左75%) + サイドバー(右25%) | main, sidebar |
| `grid-4` | 4分割 | grid1(左上), grid2(右上), grid3(左下), grid4(右下) |

テロップは画面全体にオーバーレイ表示され、レイアウトに関係なく常に画面の上下いずれかに表示されます。

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

- **画面レイアウト**：使いたいレイアウトパターンをクリックして選択
- **テロップ文章**：テキストエリアを直接書き換える
- **テロップON/OFF**：トグルボタンで切り替え
- **スクロール速度**：数値を変更（大きいほど速い）
- **ゾーンごとの動画**：
  - 追加：各ゾーンの下部フォームにYouTube限定公開URLを入力して「追加する」
  - 削除：各アイテムの「✕」ボタン
  - 並び替え：左端の「↕」をドラッグ（同一ゾーン内のみ）
- **画像の表示秒数**：各ゾーンごとに設定可能

**④ config.json を保存する**

「↓ config.json を保存」ボタンをクリック
→ `config.json` がダウンロードフォルダに保存される

**⑤ Google Drive に上書きアップロード**

1. Google Drive を開く：https://drive.google.com
2. 「マイドライブ」→「firesign」フォルダを開く
3. ダウンロードした `config.json` をフォルダにドラッグ＆ドロップ
4. 「既存のファイルを置き換えますか？」→「置き換える」をクリック

**⑥ プレーヤーへの反映を確認**

- プレーヤーは **60秒ごと** に自動で設定を再読み込みする（config.json だけでなく画像フォルダの中身も再スキャンする）
- すぐに反映させたい場合はプレーヤーのページを再読み込みする（FireTV: ホームボタン→Silkブラウザを再起動）

---

## コンテンツ（画像）をGoogle Driveから表示する

画像はすべてのゾーンで **同じ1つの firesign フォルダ** を共有します。ファイル名の先頭に「ゾーンID_」を付けることで、どのゾーンに表示されるかが決まります。ファイル名のアルファベット順に自動で再生されます。

### 手順

**① ファイル名にゾーンIDを付けてアップロード**

Google Drive の `firesign` フォルダに、以下のような名前で画像（jpg/png）をアップロードします。

```
main_01_welcome.jpg     ← main ゾーンで1番目に再生
main_02_menu.jpg        ← main ゾーンで2番目に再生
sidebar_01_notice.png   ← sidebar ゾーンで再生
grid1_a.jpg             ← grid1 ゾーンで再生
```

- ゾーンIDは大文字小文字を区別しません
- 「ゾーンID_」で始まらないファイルはどのゾーンにも表示されません
- 選択中のレイアウトに存在しないゾーンIDのファイルは無視されます（例: `full` レイアウトなら `main_` のみ有効）

**② ファイルを「リンクを知っている全員」に共有**

1. ファイルを右クリック →「共有」
2. 「一般的なアクセス」を「リンクを知っている全員」に変更
3. 「完了」をクリック

（フォルダ全体を最初から共有設定しておけば、以後アップロードするファイルは個別設定不要です）

---

## 動画コンテンツ（YouTube限定公開）

v2.0 から動画はDrive直リンクではなく **YouTube「限定公開」** URLを使用します。

**① YouTubeに動画をアップロードし、公開設定を「限定公開」にする**

**② 動画URLを管理画面の各ゾーンの「YouTube動画を追加」フォームに入力する**

```
https://youtu.be/XXXXXXXXXXX
```
または
```
https://www.youtube.com/watch?v=XXXXXXXXXXX
```

**③ config.json を保存してDriveにアップロード**（上記の手順参照）

### ⚠ FireTV Stick での動画再生について

FireTV Stick（特に無印・Liteモデル）はハードウェアの動画デコーダが非力です。複数のゾーンで同時にYouTube動画を再生すると、映像が乱れたり、ブラウザが重くなる可能性があります。

- 可能な限り、動画は1画面につき同時に1ゾーンまでに留めることを推奨します
- 複数ゾーンに動画を配置する場合は、実機（実際に使うFireTV Stickの機種）で必ず動作確認してください
- 動作が重い場合は、動画を配置するゾーンを減らす、またはレイアウトを `full` や `split-lr` など少ないゾーン数に変更してください

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

### ⚠ 常時稼働させる場合は必ずスリープ・スクリーンセーバーを解除すること

サイネージ用途では画面が自動で暗くなったりスリープしないよう、以下を必ず設定してください。

1. FireTV ホーム画面 →「設定」→「ディスプレイとサウンド」→「スクリーンセーバー」→「開始までの時間」を「なし」に設定
2. 「設定」→「ディスプレイとサウンド」→「スリープ状態にするまでの時間」を「なし」に設定
3. これを設定しないと、一定時間操作がないと画面が暗転・スクリーンセーバー表示になりサイネージが止まります
4. Silk ブラウザ側の省電力設定がある場合も無効化してください

---

## PC（ブラウザ）で動作確認する場合

`index.html` / `firesign_admin.html` はどちらも静的ファイルで、特別なインストール作業は不要です。GitHub Pages のURL（https://take4-minami.github.io/firesign/ ）をどのPC・どのブラウザ（Chrome, Edge, Safariなど）で開いても同じように動作します。職場PC・自宅PCなど環境を問わず、同じURLにアクセスするだけでテストできます。ローカルにファイルを置いて `file://` で開く場合、ブラウザによってはCORSの制限でDrive APIの呼び出しがブロックされることがあるため、基本は GitHub Pages 経由でのアクセスを推奨します。

---

## トラブルシューティング

| 症状 | 原因 | 対処 |
|---|---|---|
| 画面が真っ黒のまま | ネットワーク未接続 | FireTVのWi-Fi設定を確認 |
| テロップが古いまま | Drive同期待ち | 60秒待つかブラウザを再読み込み |
| 「画像の読み込みに失敗」 | URLが間違っている、共有設定がされていない | Driveのファイル共有設定を確認 |
| 管理画面でDrive読込失敗 | APIキーの制限 | Google Cloud ConsoleでAPIキーを確認 |
| 特定ゾーンに画像が出ない | ファイル名のゾーンID prefixが間違っている | ファイル名が `ゾーンID_` で始まっているか確認 |
| 動画が重い・フリーズする | 複数ゾーンで同時にYouTube再生 | 動画ゾーン数を減らす、レイアウトを変更する |
| 一定時間で画面が暗くなる | FireTVのスリープ/スクリーンセーバー設定 | 上記「スリープ解除設定」を実施 |

---

## ファイル構成

```
GitHub (take4-minami/firesign)
├── index.html          ← プレーヤー本体（マルチゾーン対応）
├── firesign_admin.html ← 管理画面（レイアウト・ゾーン設定対応）
└── README.md            ← このマニュアル

Google Drive (firesign フォルダ)
├── config.json               ← 設定ファイル（管理画面で編集）
└── ゾーンID_*.jpg / *.png     ← 画像コンテンツ（ゾーンIDプレフィックス必須）
```

---

## config.json の構造（v2.0）

```json
{
  "layout": "split-lr",
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
  "zones": {
    "left": {
      "imageDuration": 5,
      "videos": [
        { "url": "https://youtu.be/XXXXXXXXXXX" }
      ]
    },
    "right": {
      "imageDuration": 5,
      "videos": []
    }
  },
  "loop": true,
  "errorRetry": 10
}
```

v1.0 の `playlist` 配列（画像URLを手動登録する方式）は廃止されました。画像はDriveフォルダのファイル名スキャンに、動画（mp4直リンク）はYouTube限定公開URLに置き換わっています。

---

## Claude「プロジェクト」との使い分けについて

Claude の「プロジェクト」機能に途中まで書きかけのFireSignがある場合、**GitHubリポジトリ（https://github.com/Take4-Minami/firesign ）を唯一の正とする**ことを推奨します。理由：

- Cowork（このセッション）はプロジェクトの中身を直接参照・同期できません
- 複数の場所でファイルを編集すると、どちらが最新か分からなくなり事故のもとになります

運用案：
1. 今回 Cowork で作成した `index.html` / `firesign_admin.html` / `README.md` を GitHub に反映する（コミット・プッシュ、またはGitHub Web UIでアップロード）
2. Claude「プロジェクト」側の書きかけファイルは、GitHub の最新版で上書きするか、参照用に「アーカイブ済み」であることをメモしておく
3. 以後の編集は GitHub 上のファイルを基準にし、Cowork でもプロジェクトでも「まずGitHubから最新を取得してから編集する」運用に統一する

---

*FireSign — オープンソース デジタルサイネージシステム*
