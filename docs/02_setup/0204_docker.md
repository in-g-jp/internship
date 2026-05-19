# 0204 Docker Desktop のセットアップ

## Docker とは

アプリケーションを **コンテナ** という独立した箱の中で動かす仕組みです。

「自分の PC では動くのに他の人の PC では動かない」という問題が開発現場でよく起きます。
Docker を使うと全員が同じ環境でアプリを動かせるため、この問題を防ぐことができます。

このプロジェクトでは **Laravel Sail** というツールを通じて Docker を使います。
05章でLaravelのセットアップをするときに実際に使います。

## Docker Desktop とは

Mac で Docker を使うために必要なアプリです。

---

## 1. インストール

[Docker Desktop 公式サイト](https://www.docker.com/products/docker-desktop/) から Mac 版をダウンロードしてインストールしてください。

インストール後、Docker Desktop を起動してください。

```bash
docker --version
# Docker version 29.2.1, build a5c7197
```

---

## チェックリスト

- [ ] Docker Desktop が起動している
- [ ] `docker --version` が表示される
