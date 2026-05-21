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
laravel new prefecture-capital-api
```

コマンドを実行するといくつか質問されます。以下の通り選択してください。

```
 ┌ Which starter kit would you like to install? ────────────────┐
 │ None                                                         │
 └──────────────────────────────────────────────────────────────┘
```

```
 ┌ Which testing framework do you prefer? ──────────────────────┐
 │ PHPUnit                                                      │
 └──────────────────────────────────────────────────────────────┘
```

```
 ┌ Do you want to install Laravel Boost to improve AI assisted coding? ┐
 │ No                                                                  │
 └─────────────────────────────────────────────────────────────────────┘
```

```
 ┌ Which database will your application use? ───────────────────┐
 │ PostgreSQL                                                   │
 └──────────────────────────────────────────────────────────────┘
```

```
 ┌ Default database updated. Would you like to run the default database migrations? ┐
 │ No                                                                               │
 └──────────────────────────────────────────────────────────────────────────────────┘
```

```
 ┌ Would you like to run npm install --ignore-scripts and npm run build? ┐
 │ No                                                                    │
 └───────────────────────────────────────────────────────────────────────┘
```

---

## 4. Laravel Sail のインストール

```bash
cd prefecture-capital-api
composer require laravel/sail --dev
php artisan sail:install
```

> 📖 [Laravel Sail（公式ドキュメント）](https://laravel.com/docs/13.x/sail)

サービスの選択肢が表示されるので、デフォルトでチェックが入っている `mysql` を外し、`pgsql` のみを選択してください。
(スペースを押下すると選択を切り替えられます。)

```
 ┌ Which services would you like to install? ───────────────────┐
 │ pgsql                                                        │
 └──────────────────────────────────────────────────────────────┘
```

---

## 5. GitHub リポジトリの作成

[GitHub](https://github.com) にログインし、新しいリポジトリを作成してください。

| 項目 | 値 |
|------|------|
| Repository name | `prefecture-capital-api` |
| Visibility | Public |
| Initialize this repository | チェックしない |

---

## 6. 最初のコミットとプッシュ

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
| `git remote add` | ローカルリポジトリとリモートリポジトリ（GitHub）を接続するコマンド |

### 参考資料

- [Laravel インストール（公式ドキュメント）](https://laravel.com/docs/13.x/installation)
- [Laravel Sail 公式ドキュメント](https://laravel.com/docs/13.x/sail)
