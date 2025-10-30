# zenn_article

Zenn CLIを使用した技術記事・本の管理リポジトリです。

## ディレクトリ構成

```
.
├── articles/           # 技術記事（.mdファイル）
├── books/              # 本（複数チャプターで構成）
│   └── [book-id]/      # 各本のディレクトリ
│       ├── config.yaml
│       └── *.md        # チャプター
├── package.json
└── package-lock.json
```

## セットアップ

zenn-cli install
```bash
$ npm init --yes
$ npm install zenn-cli 
```

初期化
```bash
$ npx zenn init 
```

更新
```bash
$ npm install zenn-cli@latest
```

## 主要コマンド

### プレビュー
```bash
npx zenn preview
```
ブラウザでローカルプレビューを表示（デフォルト: http://localhost:8000）

### 新規記事作成
```bash
npx zenn new:article
```
`articles/`ディレクトリに新しい記事ファイルを生成

### 新規本作成
```bash
npx zenn new:book
```
`books/`ディレクトリに新しい本のフォルダを生成

### その他
- `npx zenn --help` - ヘルプを表示

## 公開方法

GitHubリポジトリとZennを連携することで、`main`ブランチへのpushで自動的に記事が公開されます。

詳細: https://zenn.dev/zenn/articles/connect-to-github