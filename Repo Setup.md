# Gitリポジトリ登録方法（20260201）

aoki aaa

## 1. 習得した主要コマンドリスト
本日の検証で使用したコマンドと、エンジニアとして押さえておくべき意味のまとめです。

| コマンド | 意味 | プロの補足 |
| :--- | :--- | :--- |
| `git init` | Gitリポジトリの初期化 | `.git`フォルダが作成され、管理が開始される。 |
| `ls -al` | 全ファイルの詳細表示 | 隠しファイル（`.git`や`.DS_Store`）を確認するために必須。 |
| `git status` | 状態確認 | 作業の区切りごとに必ず打つ癖をつけること。 |
| `git add .` | ステージング（全対象） | 変更された全てのファイルを「コミット予定」にする。 |
| `git commit -m "msg"` | コミット（保存） | メッセージと共にローカルに歴史を刻む。 |
| `git remote add origin [URL]` | リモート登録（新規） | GitHubの宛先を `origin` という名で登録する。 |
| `git remote set-url origin [URL]` | リモート登録（上書き） | 既存の `origin` の宛先だけを変更する（エラー回避に有効）。 |
| `git remote -v` | 接続先確認 | 登録されたURLが正しいか確認する。 |
| `git push -u origin main` | 初回プッシュ | `-u` で紐付けを行うと、次回から `git push` だけで済む。 |
| `rm -rf [dir]` | フォルダ強制削除 | 検証環境のリセット用。実行時は現在地に注意。 |

---

## 2. 実験と検証の記録（Case Study）

### 実験①：`test` リポジトリの作成（基本フロー）
- **目的**：最初のGitHub連携テスト。
- **手順**：`git init` → `git add` → `git commit` → `git remote add` → `git push`。
- **結果**：成功。`index.html` がGitHubへ正しく反映された。

### 実験②：`test2` での重複エラー検証（トラブルシュート）
- **現象**：`git remote add origin ...` を実行した際、以下のエラーが発生。
  ```text
  error: remote origin already exists.

原因：
ログに Reinitialized existing Git repository とある通り、過去にこのフォルダで git init を実行済みだった。
そのため、古い origin 設定が .git/config 内に残っており、新規登録と衝突した。

学びと解決策：
原則：設定はフォルダ（.git）ごとに独立して管理されている。
解決策：既存の設定がある場合は add（追加）ではなく set-url（上書き）を使うのがプロの定石。
コマンド：git remote set-url origin [新しいURL]

実験③：test3 でのクリーンな構築（ベストプラクティス）
手順：
新規フォルダ作成 (mkdir test3)
初期化 (git init)
ファイル作成 (README.md)

記録 (git commit)
紐付けと送信 (git push -u origin main)
結果：成功。
成果：過去のゴミファイルや設定に邪魔されない、最も綺麗な手順を確立。

3. 発生したトラブルと解決策
⚠️ コマンド入力ミス
エラー：-bash: remote: command not found
原因：git remote ... と打つべきところを、先頭の git を忘れて remote ... と入力したため。
対策：コマンドは必ず git から始まることを意識する。

⚠️ Mac特有の不要ファイル混入
現象：git add . をした際、.DS_Store というMacのシステムファイルもステージングされてしまった。
対策：プロジェクト開始直後に .gitignore ファイルを作成し、中に .DS_Store と記述してから git add を行う。

⚠️ ユーザー名設定の警告
現象：初回コミット時に、Gitがユーザー名とEmailを自動設定した旨のメッセージが表示された。
対策：以下のコマンドで自分の署名を固定する。

Bash
git config --global user.name "Satoshi Sugizaki"
git config --global user.email "your_email@example.com"



