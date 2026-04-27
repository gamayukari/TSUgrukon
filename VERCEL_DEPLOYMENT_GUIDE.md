# Vercelへのアップロード手順書

グルコン講座のアンケート分析レポートをVercelにデプロイするための詳細ガイドです。

---

## 📋 準備する情報

1. **GitHubアカウント** （新規作成の場合は先に登録）
2. **Vercelアカウント** （GitHubでログイン可能）

---

## 🚀 方法A：GitHubリポジトリを作成 → Vercelで自動デプロイ（推奨）

### ステップ1: GitHubでリポジトリを作成

1. [GitHub](https://github.com) にログイン
2. 右上の **「+」** → **「New repository」** をクリック
3. 以下を入力：
   - **Repository name**: `glukkon-survey-report`
   - **Description**: `グルコン講座 アンケート分析レポート`
   - **Public** を選択
   - **README file を追加** にチェック

4. **Create repository** をクリック

### ステップ2: ローカルマシンで作業

```bash
# リポジトリをクローン
git clone https://github.com/YOUR_USERNAME/glukkon-survey-report.git
cd glukkon-survey-report

# ファイルをコピー（ダウンロードしたファイルを配置）
# 以下のファイルをディレクトリにコピー：
# - index.html
# - vercel.json
# - package.json
# - README.md

# Gitに追加してコミット
git add .
git commit -m "初期コミット: グルコン講座アンケート分析レポート"
git push origin main
```

### ステップ3: VercelでGitHubリポジトリを接続

1. [Vercel](https://vercel.com) にアクセス
2. **GitHubでサインアップ** または **ログイン**
3. **Dashboard** → **New Project** をクリック
4. **Import Git Repository** でGitHubリポジトリを検索・選択
5. 以下の設定で進める：
   - **Project Name**: `glukkon-survey-report`
   - **Framework Preset**: **Other**
   - **Build Command**: `echo 'No build needed'`
   - **Output Directory**: `.`
   - **Environment Variables**: （なし）

6. **Deploy** をクリック

### ステップ4: デプロイ完了

数秒後に自動デプロイが完了し、以下のような形式のURLが発行されます：

```
https://glukkon-survey-report.vercel.app
```

このURLをブラウザで開いてレポートが表示されることを確認してください。

---

## 🚀 方法B：Vercel CLIを使用

### ステップ1: Vercel CLIをインストール

```bash
npm install -g vercel
```

### ステップ2: 初回ログイン

```bash
vercel login
```

Vercelアカウント認証が求められるので、ブラウザで認証を完了してください。

### ステップ3: プロジェクトをデプロイ

```bash
# プロジェクトディレクトリに移動
cd /path/to/project

# デプロイを実行
vercel
```

プロンプトに従って以下を入力：

```
? Set up and deploy "~/glukkon-survey-report"? [Y/n] Y
? Which scope do you want to deploy to? [Your Account]
? Link to existing project? [y/N] N
? What's your project's name? glukkon-survey-report
? In which directory is your code located? . (current directory)
? Want to modify these settings? [y/N] N
```

### ステップ4: デプロイ完了

```
✓ Production: https://glukkon-survey-report.vercel.app
```

このURLがデプロイされたレポートのアドレスです。

---

## 🔄 更新・変更方法

### 方法A（GitHub統合）の場合

```bash
# ローカルで変更を加える
# （HTMLやデータを編集）

# Gitにコミット
git add .
git commit -m "レポート更新: XXXを変更"
git push origin main

# 自動的にVercelがデプロイを開始
# 数秒後に本番環境に反映
```

### 方法B（Vercel CLI）の場合

```bash
# 変更を加える
# （HTMLやデータを編集）

# 再度デプロイ
vercel --prod
```

---

## 📊 データ更新の手順

アンケート結果を更新する場合：

### 1. 満足度スコアを更新

`index.html` 内の以下の部分を探して、データを変更：

```javascript
// 約570行目付近
datasets: [{
  label: '人数',
  data: [25, 9, 4, 2],  // ← [5点の人数, 4点, 3点, 2点]
  // ...
}]
```

### 2. テーマ別出現頻度を更新

```javascript
// 約620行目付近
datasets: [{
  label: '学んだ人数（複数回答）',
  data: [18, 18, 16, 15, 14, 12, 10],  // ← 各テーマの出現数
  // ...
}]
```

### 3. メトリクス数値を更新

```html
<!-- 約130行目付近 -->
<div class="metric-value">4.4</div>  <!-- 平均満足度 -->
<div class="metric-value">40</div>   <!-- 回答者数 -->
<div class="metric-value">85%</div>  <!-- 高満足度率 -->
```

---

## 🎯 カスタマイズ例

### タイトルを変更

```html
<!-- index.html 内 -->
<h1>📊 グルコン講座 アンケート分析レポート</h1>
<!-- ↓ 変更後 -->
<h1>📊 新年度 講座生満足度レポート</h1>
```

### カラースキームを変更

```css
/* index.html内の:rootセクション */
:root {
  --primary-color: #185FA5;        /* これを別の色に変更 */
  --success-color: #1D9E75;        /* これを別の色に変更 */
  /* ... */
}
```

推奨色の例：
- プライマリ: `#0066CC`（濃いブルー）
- サクセス: `#22C55E`（グリーン）
- ワーニング: `#F59E0B`（アンバー）

---

## 🔒 セキュリティ設定

### プライベートURLにする場合

Vercelダッシュボードで：

1. **Settings** → **Deployment Protection**
2. **Password protection** を有効化
3. パスワードを設定

※ リンクを知っている人なら誰でもアクセスできるため、本当にプライベートにしたい場合は別途検討が必要です。

---

## 📱 プレビューと本番環境

Vercelでは以下の環境が自動的に作成されます：

### 本番環境（Production）
```
https://glukkon-survey-report.vercel.app
```

### プレビュー環境（Git Branch）
```
https://glukkon-survey-report-[branch-name].vercel.app
```

GitHubで別ブランチを作成して更新を試すと、Vercelが自動的にプレビューURLを生成します。

---

## 🐛 トラブルシューティング

### Q: デプロイに失敗した

**A:** 以下を確認：

1. `index.html` が存在するか確認
2. ファイルの形式が正しいか確認（BOMなし、UTF-8エンコーディング）
3. `vercel.json` のフォーマットが正しいか確認

```bash
# JSONの形式チェック
python -m json.tool vercel.json
```

### Q: ページが真っ白に表示される

**A:** ブラウザのコンソールを確認（F12キー）

1. JavaScript エラーが出ていないか確認
2. ネットワークタブで外部リソース（Chart.jsなど）が正しく読み込まれているか確認

### Q: グラフが表示されない

**A:** 以下を確認：

1. Chart.jsがCDNから正しく読み込まれているか確認
2. ブラウザのコンソールでエラーがないか確認
3. キャッシュをクリア（Ctrl+Shift+Delete）してリロード

### Q: データの更新が反映されない

**A:**

1. GitHubに正しくプッシュされたか確認
2. Vercelダッシュボードで新しいデプロイが開始されたか確認
3. デプロイ完了後、ブラウザキャッシュをクリアしてリロード

---

## 📞 更なるサポート

### Vercelのドキュメント
- https://vercel.com/docs

### Git/GitHub初心者向け
- https://docs.github.com/ja/get-started

### Chart.jsドキュメント
- https://www.chartjs.org/docs/latest/

---

## ✅ チェックリスト

デプロイ前に以下を確認してください：

- [ ] 4つのファイルが揃っている（index.html, vercel.json, package.json, README.md）
- [ ] GitHubアカウントを持っている
- [ ] Vercelアカウントを持っている（GitHubでサインアップ可）
- [ ] インターネット接続がある
- [ ] ブラウザで最新版を使用している

---

**制作日**: 2026年4月26日

これでVercelへのアップロードが完全に可能です！🚀

