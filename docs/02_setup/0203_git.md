# 0203 Git セットアップ

## 1. Git の初期設定

Git にユーザー情報を登録します。コミット履歴に表示される名前とメールアドレスです。

```bash
git config --global user.name "あなたの名前"
git config --global user.email "あなたのメールアドレス"
```

設定を確認してください。

```bash
git config --list
```

---

## 2. SSH キーの生成

GitHub との通信に使う SSH キーを生成します。

```bash
ssh-keygen -t ed25519 -C "あなたのメールアドレス"
```

ファイルの保存場所とパスフレーズを聞かれますが、そのまま Enter を押して進めて問題ありません。

---

## 3. SSH キーを GitHub に登録する

生成した公開鍵を表示してコピーしてください。

```bash
cat ~/.ssh/id_ed25519.pub
```

[GitHub の SSH 設定ページ](https://github.com/settings/ssh/new) を開き、以下の手順で登録してください。

1. 「Title」に任意の名前を入力（例: `my-mac`）
2. 「Key」にコピーした内容を貼り付け
3. 「Add SSH key」をクリック

---

## 4. 接続確認

```bash
ssh -T git@github.com
```

以下のように表示されれば成功です。

```
Hi <ユーザー名>! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## チェックリスト

- [ ] `git config --list` でユーザー名とメールアドレスが表示される
- [ ] `ssh -T git@github.com` で認証成功のメッセージが表示される

---

## 学習

### 今何をしたか

Git の初期設定を行い、GitHub に SSH キーを登録しました。

| 用語 | 説明 |
|------|------|
| `git config` | Git の設定を変更するコマンド |
| SSH キー | サーバーへの接続に使う認証キー。公開鍵と秘密鍵のペアで構成される |
| 公開鍵 | GitHub に登録する鍵。外部に公開して問題ない |
| 秘密鍵 | 手元の PC にのみ保管する鍵。外部に漏らしてはいけない |

### 参考資料

- [Git 入門（サル先生のGit入門）](https://backlog.com/ja/git-tutorial/)
- [GitHub への SSH 接続（GitHub 公式）](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh)
