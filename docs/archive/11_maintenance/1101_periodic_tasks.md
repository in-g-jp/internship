# 定期メンテナンス

開発環境を最新の状態に保つための定期実行コマンド。

## npm グローバルパッケージの更新

更新対象を確認する。

```bash
ncu -g
```

表示されたパッケージを個別にインストールする。

```bash
npm install -g <package-name>
```

## Composer グローバルパッケージの更新

```bash
composer global update
```

## Homebrew の更新

```bash
brew update && brew upgrade && brew autoremove && brew cleanup --prune=all && brew doctor
```

## Node.js のマイナーアップデート

nvm で現在利用中のメジャーバージョン内の最新版をインストールする。
`--reinstall-packages-from=current` を付けると npm グローバルパッケージが新バージョンへ引き継がれる。

```bash
nvm install <major-version> --reinstall-packages-from=current && nvm alias default <major-version>
```

例（22系の場合）:

```bash
nvm install 22 --reinstall-packages-from=current && nvm alias default 22
```
