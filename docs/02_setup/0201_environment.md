# 0201 環境構築（CLI ツール）

以下の手順を進めてください。

---

## 1. Warp（ターミナル）

ターミナルアプリとして **Warp** をおすすめします。
コマンドの補完や履歴検索が使いやすく、開発効率が上がります。

[https://www.warp.dev/](https://www.warp.dev/) からダウンロードしてインストールしてください。

以降の手順はすべて Warp で実施してください。

---

## 2. Homebrew

[https://brew.sh/ja/](https://brew.sh/ja/) に記載のコマンドをターミナルに貼り付けて実行してください。

```bash
brew --version
# Homebrew 5.0.16
```

---

## 3. Git

Mac にすでに入っている場合があります。まず確認してください。

```bash
git --version
# git version 2.53.0
```

表示されない場合はインストールしてください。

```bash
brew install git
```

---

## 3. PHP

```bash
brew install php
```

```bash
php --version
# PHP 8.5.3 (cli) ...
```

---

## チェックリスト

- [ ] `brew --version` が表示される
- [ ] `git --version` が表示される
- [ ] `php --version` が表示される

---

## 学習

### 今何をしたか

開発に必要な CLI ツールをセットアップしました。

| ツール | 役割 |
|--------|------|
| Homebrew | Mac にツールを簡単にインストールできるパッケージマネージャー |
| Git | コードの変更履歴を管理するバージョン管理ツール |
| PHP | Laravel（バックエンド）を動かすプログラミング言語 |

### 参考資料

- [Git 入門（サル先生のGit入門）](https://backlog.com/ja/git-tutorial/)
