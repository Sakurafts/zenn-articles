---
title: "Next.jsアプリをCloudflare Pagesにデプロイして直面したEdge Runtimeの壁とDB(D1)移行"
emoji: "☁️"
type: "tech"
topics: ["nextjs", "cloudflare", "cloudflarepages", "d1", "個人開発"]
published: false
---

AIと共同開発したフルスタック食品管理アプリ「[Pantry Quest](https://pantry-quest.sakurapotions.com)」をリリースするにあたり、ホスティング先として**Cloudflare Pages**を採用しました。

しかし、Next.js (App Router) を Cloudflare Pages の Edge 環境へデプロイする中で、**Edge Runtime特有の依存関係エラー**や、**データベース（D1）への移行問題**など、インフラ寄りの泥臭いトラブルに多数直面しました。

この記事では、本編である「[アプリ開発未経験者がAIと3ヶ月でフルスタックアプリを作った話（※公開後リンク）]()」では泣く泣くカットした、**Cloudflareへのデプロイに向けた技術的なトラブルシューティング（インフラ・環境構築編）**をまとめます。

---

## 1. Cloudflare Pages 最大の罠：Edge Runtimeと `async_hooks` エラー

ローカル環境では問題なく動いていたアプリが、Cloudflare Pagesにデプロイした途端に `500 Internal Server Error` を吐くようになりました。

ログを確認すると、Node.jsのモジュール群（特に `async_hooks` など）に関するエラーが発生していることが判明しました。Cloudflare Pagesの実行環境（Edge Runtime）は、通常のNode.jsフル環境とは異なるためです。

### 解決策： `nodejs_compat` の有効化
（※ここに Cloudflareダッシュボードでの互換性フラグ設定や、ファイルでの指定方法を記載する）

### Webpack Alias の解消
`nodejs_compat` フラグを有効にしても、Next.jsのビルド・バンドル時にモジュール解決エラーが出るケースへの対応。
（※ `next.config.mjs` 等における Webpack の `alias` 設定をどのように上書き・整理したかを記載）

---

## 2. データベースの引越し：PostgreSQL から Cloudflare D1(SQLite) へ

Edge環境への完全移行にあたり、データベースもCloudflareエコシステム（D1）に乗せ換える必要性が生まれました。

### 泥臭いスキーマとデータのマイグレーション
元々使用していたNeon（PostgreSQL）から、Cloudflare D1（SQLite）への移行作業の難所。
（※ `export_pg_to_sqlite.ts` 等のスクリプトを使ったデータ変換や、SQLの互換性の壁について記載）

### D1特有の制約とORMの対応
（※ D1で引っ掛かった制約や、Prisma連携における Edge 環境特有の注意点などを記載）

---

## 3. 本番公開に向けた残りのピース（AdSense等）

ただ動かすだけでなく、アプリとして公開するための周辺設定についても触れておきます。
（※ マネタイズのための Adsense 設定や、それに伴うサイト構造・ポリシー設定などについて記載）

---

## まとめ：AI時代でも「インフラの基本概念」からは逃げられない

AIは「とりあえず動くコード」を恐ろしい速度で出力してくれますが、**「本番環境（Edge）とローカル（Node.js）の違い」**や**「DBアーキテクチャの違い」**までは勝手に調整してくれません。

「なぜ動かないのか」をAIに質問するにしても、自分自身にある程度のインフラや環境の概念がなければ、適切なプロンプトすら打てないことを痛感しました。

同じようにNext.jsをCloudflare Pagesにデプロイして謎のエラーにハマっている方へ、この記事が少しでも解決のヒントになれば幸いです！
