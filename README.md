# 送迎登録アプリ

毎週の送迎登録をリアルタイムで共有できるWebアプリです。

---

## ファイル構成

```
sogei-app/
├── index.html          # アプリ本体（フォーム＋一覧）
├── firebase-config.js  # Firebase設定（要編集）
└── README.md
```

---

## セットアップ手順

### 1. Firebaseプロジェクトの作成

1. [Firebase Console](https://console.firebase.google.com/) を開く
2. **「プロジェクトを追加」** をクリック
3. プロジェクト名を入力（例: `sogei-app`）して作成

---

### 2. Realtime Database の有効化

1. 左メニューの **「構築」→「Realtime Database」** を選択
2. **「データベースを作成」** をクリック
3. リージョンは **「asia-southeast1（シンガポール）」** を選択（日本に近い）
4. セキュリティルールは **「テストモードで開始」** を選択
5. 作成後、**「ルール」タブ** で以下に変更して公開（30日制限を外す）：

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> ⚠️ このルールは全員が読み書きできる設定です。身内での利用に限定してください。

---

### 3. firebase-config.js への設定値の記入

1. Firebase Console の **「プロジェクトの概要」→ 歯車アイコン→「プロジェクトの設定」** を開く
2. **「全般」タブ** を下にスクロールし、**「マイアプリ」** セクションへ
3. **「ウェブ」アイコン（`</>`）** をクリックしてウェブアプリを登録
4. アプリ名を入力（「Firebase Hosting も設定する」にチェック）して登録
5. 表示された `firebaseConfig` の値を `firebase-config.js` にコピー：

```js
// firebase-config.js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

> `databaseURL` は Realtime Database の画面に表示されているURLをコピーしてください。

---

### 4. Firebase Hosting へのデプロイ

#### 事前準備（初回のみ）

```bash
# Node.js が必要です（インストール済みであること）
npm install -g firebase-tools

# Googleアカウントでログイン
firebase login
```

#### デプロイ手順

```bash
# sogei-app フォルダに移動
cd /path/to/sogei-app

# Firebase プロジェクトを初期化
firebase init hosting
```

初期化の質問への回答：
- **Which Firebase project?** → 作成したプロジェクトを選択
- **What do you want to use as your public directory?** → `.`（ドットを入力）
- **Configure as a single-page app?** → `No`
- **Set up automatic builds with GitHub?** → `No`
- **File ./index.html already exists. Overwrite?** → `No`

```bash
# デプロイ実行
firebase deploy --only hosting
```

完了すると以下のようなURLが表示されます：

```
Hosting URL: https://your-project.web.app
```

このURLを全員に共有してください。

---

### 5. LINEグループへのURL共有方法

1. 上記の `https://your-project.web.app` をコピー
2. LINEグループで貼り付けて送信
3. メンバーはスマホのブラウザで開いて使用できます

> iPhone でホーム画面に追加すると、アプリのように使えて便利です。
> Safari で開いて「共有」→「ホーム画面に追加」を選択してください。

---

## 使い方

| 操作 | 説明 |
|------|------|
| 名前を入力 | テキストボックスに名前を入力 |
| 場所を選択 | 「大学」か「駅」をタップ |
| 登録する | ボタンを押すと全員のリストにリアルタイム反映 |
| 削除 | 各エントリの「削除」ボタン |
| 全件リセット | 管理者用ボタン（一番下） |

- 同じ名前の重複登録はできません
- 毎週月曜日（ISO週基準）に自動でリセットされます
