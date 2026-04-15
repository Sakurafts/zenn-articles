# Zenn Articles Workspace

ZennのGitHub連携（Sakuraftsアカウント）を用いた記事管理用リポジトリです。

## 📝 基本的な使い方

### 1. 新しい記事の作成
\`\`\`bash
npx zenn new:article
\`\`\`
自動で `articles/` フォルダの中にマークダウンファイル（ランダムな英数字のファイル名）が生成されます。
ファイル名（スラグ）は記事のURLになるため、必要に応じて変更してください。（※半角英数字とハイフンのみ使用可能）

### 2. 手元でプレビュー（確認）する
\`\`\`bash
npx zenn preview
\`\`\`
ブラウザで [http://localhost:8000](http://localhost:8000) が開き、Zennの画面と全く同じデザインでリアルタイムで記事を確認できます。

### 3. Zennへ公開・反映する
記事ファイルの最初の設定（フロントマター）にある `published:` を変更します。
- `published: false` ➔ 「下書き」状態（Zennの管理画面で自分だけ見れます）
- `published: true`  ➔ 「公開」状態

GitにコミットしてPushすると、自動的にZennへと反映されます！
\`\`\`bash
git add .
git commit -m "Update article"
git push origin main
\`\`\`

---
## 💡 開発メモ・注意点

### Gitのプッシュ時に認証エラーが出る場合
ターミナルから `git push` を行った際、「Device not configured」などのエラーでプッシュできない場合があります。（GitHubのパスワード認証廃止とMacのキーチェーン設定にまつわる仕様です）
その際はこのワークスペースのルートにある `memo.txt` のGitHub Personal Access Token（pantry-quest用と同じもの）を使用して認証するか、以下のように一時的にURLにトークンを埋めてプッシュを行ってください。

\`\`\`bash
git remote set-url origin https://<YOUR_TOKEN>@github.com/Sakurafts/zenn-articles.git
git push origin main
git remote set-url origin https://github.com/Sakurafts/zenn-articles.git
\`\`\`

### 参考リンク
* [Zenn CLIの使い方ガイド](https://zenn.dev/zenn/articles/zenn-cli-guide)