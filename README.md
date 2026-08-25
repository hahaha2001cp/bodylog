# BODY LOG — 身体計測記録（隔週）

Mac の Dock から1クリックで起動し、オフラインでも動く記録アプリです。
記録はこの Mac の中にのみ保存され、外部に送信されません。

**Mac の Google Chrome で使ってください。**（理由は下記「必ず Chrome を使ってください」）

## 構成

| ファイル | 役割 |
|---|---|
| index.html | アプリ本体（依存ライブラリなし・単一ファイル） |
| sw.js | オフライン動作用 |
| manifest.webmanifest | ホーム画面追加の設定 |
| icon-180/192/512.png | アイコン |

## 公開方法A：GitHub の画面だけで完結（ターミナル不要）

1. github.com でアカウントを作成し、緑の **New** から新しいリポジトリを作る
   - Repository name: `bodylog`
   - **Public** を選ぶ（Private は GitHub Pages が使えない場合があります）
   - **Create repository**
2. **uploading an existing file** を押し、この6ファイルをまとめてドラッグして **Commit changes**
3. **Settings** → 左メニュー **Pages** → Source を **Deploy from a branch**、
   Branch を **main / (root)** にして **Save**
4. 1〜2分待つと `https://<ユーザー名>.github.io/bodylog/` が発行される
5. Mac の **Google Chrome** でその URL を開く
6. アドレス欄の右端に出る **インストールアイコン**（画面に↓のマーク）を押す。
   見当たらないときは 右上「⋮」→ **キャスト、保存、共有** → **ページをアプリとしてインストール**
7. Dock にアイコンができるので、右クリック → **オプション** → **Dock に追加**

以後は Dock のアイコンから1クリックで起動します。オフラインでも動きます。

### 必ず Chrome を使ってください（Safari 不可）

Safari には「7日間アクセスがないサイトの保存データを削除する」仕様があります。
このアプリは隔週＝14日間隔で使うため、**Safari だと次に開く前に記録が消えます。**
Chrome にはこの制限がありません。

iPhone のホーム画面アプリは例外的に7日削除の対象外ですが、
今回は Mac を記録場所とするため、iPhone では入力しないでください。

## 公開方法B：Claude Code で管理する

改修を自分で続ける場合はこちら。Mac に Node.js が必要です。

```bash
npm install -g @anthropic-ai/claude-code
cd ~/bodylog
claude
```

Claude Code に「このアプリに◯◯を追加して」と指示すればファイルを直接編集できます。
GitHub への反映は以下。

```bash
git init && git add -A && git commit -m "initial"
git remote add origin https://github.com/<ユーザー名>/bodylog.git
git push -u origin main
```

## 更新したとき

ファイルを差し替えたら `sw.js` の 2行目 `var CACHE = 'bodylog-v3';` の
番号を上げてください（v4, v5…）。これを忘れると古い画面が残り続けます。

## バックアップ

記録は Mac の Chrome の localStorage にあります。Mac の初期化や Chrome のデータ削除で消えます。
月1回、一覧タブの **バックアップを書き出す（JSON）** からファイルを保存し、
Google ドライブなどに置いてください。復元は同じ画面の「バックアップを読み込む」から。

## 判定に使っている値

- 骨格筋量の管理下限：31.3 kg（2026/08/21 の実測値）
- 減量ペースの上限：0.8 kg/月
- 移動平均：直近3回（隔週×3＝約6週）
- 体重：前回計測日の翌日から当日までの全件平均（14件が理想）
- 身長：173 cm

これらは index.html 冒頭の定数（SMM_FLOOR / MA_N / HEIGHT_CM）で変更できます。

---
計測値の記録・集計を行うツールです。診断や治療方針の判断には使用せず、担当医の指示を優先してください。
