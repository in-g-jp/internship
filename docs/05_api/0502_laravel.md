# 0502 Laravel プロジェクト作成

## 1. Composer のインストール

```bash
brew install composer
```

```bash
composer --version
# Composer version 2.9.5 ...
```

---

## 2. Laravel installer のインストール

```bash
composer global require laravel/installer
```

---

## 3. プロジェクトの作成

```bash
laravel new prefecture-capital-api --database=pgsql
```

コマンドを実行するといくつか質問されます。以下の通り選択してください。

| 質問 | 選択 |
|------|------|
| Which starter kit would you like to install? | `No starter kit` |
| Would you like to run the default database migrations? | `No` |

---

## 4. Laravel Sail のインストール

```bash
cd prefecture-capital-api
php artisan sail:install
```

> 📖 [Laravel Sail（公式ドキュメント）](https://readouble.com/laravel/13.x/ja/sail.html)

サービスの選択肢が表示されるので `pgsql` と `mailpit` の2つを選択してください。(スペースを押下すると選択できます。)

```
Which services would you like to install? [mysql]:
  ◻︎ mysql
  ◻︎ pgsql
  ...
  ◻︎ mailpit
  ...

> Mailpit はメール送信のテストに使うツールです。実際には送信されず、ブラウザで内容を確認できます。

---

## 5. .env の確認

`.env` のデータベース設定を確認してください。
以下のようになっていれば OK です。

```env
DB_CONNECTION=pgsql
DB_HOST=pgsql
DB_PORT=5432
DB_DATABASE=laravel
DB_USERNAME=sail
DB_PASSWORD=password
```

---

## 6. GitHub リポジトリの作成

[GitHub](https://github.com) にログインし、新しいリポジトリを作成してください。

| 項目 | 値 |
|------|------|
| Repository name | `prefecture-capital-api` |
| Visibility | Public |
| Initialize this repository | チェックしない |

---

## 7. 最初のコミットとプッシュ

GitHub のリポジトリと接続して、最初のコミットをプッシュしてください。

```bash
git remote add origin https://github.com/<ユーザー名>/prefecture-capital-api.git
git add .
git commit -m "first commit"
git push -u origin main
```

---

## チェックリスト

- [ ] `prefecture-capital-api` ディレクトリに Laravel ファイル一式が作成されている
- [ ] `vendor/bin/sail` ファイルが存在している
- [ ] `.env` の `DB_CONNECTION` が `pgsql` になっている
- [ ] `docker-compose.yml` に `mailpit` が含まれている
- [ ] `git log --oneline` でコミットが確認できる
- [ ] GitHub にプッシュされている

---

## 学習

### 今何をしたか

Laravel のプロジェクトを新規作成し、Docker で動かすための Laravel Sail を導入しました。
また、コードを GitHub にプッシュしてバージョン管理を始めました。

| 用語 | 説明 |
|------|------|
| Composer | PHP のパッケージマネージャー |
| Laravel installer | `laravel new` コマンドでプロジェクトを作成できるツール |
| Laravel | PHP の Web アプリケーションフレームワーク |
| Laravel Sail | Docker を使って Laravel の開発環境を簡単に構築するツール |
| .env | アプリケーションの設定ファイル。DB 接続情報などを記載する |
| Mailpit | 開発環境でメール送信をテストするツール。実際には送信されず画面で確認できる |
| `git remote add` | ローカルリポジトリとリモートリポジトリ（GitHub）を接続するコマンド |

### 参考資料

- [Laravel インストール（公式ドキュメント）](https://laravel.com/docs/13.x/installation)
- [Laravel Sail 公式ドキュメント](https://readouble.com/laravel/13.x/ja/sail.html)
