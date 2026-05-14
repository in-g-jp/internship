# 0302 Paiza 問題への取り組み方

## 1. 取り組む問題について

主に **C問題以上** を解いてください。
問題の難易度は A（最難）〜 D の順で、C問題は実務でよく使うロジックが問われるレベルです。

---

## 2. 回答の進め方

Paiza の回答を管理する専用リポジトリがあります。以下の手順で取り組んでください。

### 2-1. 回答ディレクトリを作成する

リポジトリのルートで `make` を実行します。

```bash
$ make
```
<img width="448" height="78" alt="スクリーンショット 2026-05-14 10 21 21" src="https://github.com/user-attachments/assets/c2774eb9-4fd5-4f97-a96f-266d6e459252" />


問題番号（例: `C001`）を入力すると、`challenges/{問題番号}/{自分の名前}/` ディレクトリが自動で作成されます。

### 2-2. 回答を実装する

VS Code で作成されたディレクトリ（`challenges/問題番号/自分の名前/`）を開き、`index.php` に回答を書いてください。

<img width="406" height="361" alt="スクリーンショット 2026-05-14 10 24 19" src="https://github.com/user-attachments/assets/00bbd9eb-2ff9-4efc-a1f4-4df8cc3c944b" />


### 2-3. テストを実行する

チャレンジディレクトリに移動してから `make` を実行します。
入力ファイルを選択すると `index.php` が実行されます。

```bash
$ cd challenges/C001/ing-kamiya
$ make
```
<img width="336" height="72" alt="スクリーンショット 2026-05-14 11 16 39" src="https://github.com/user-attachments/assets/d3f83027-4901-42ee-a268-a80519e5032b" />

<img width="457" height="70" alt="スクリーンショット 2026-05-14 11 16 53" src="https://github.com/user-attachments/assets/ec2b0b42-9e02-4fe1-9460-5135a0fb09aa" />



### 2-4. ブランチを作成する

VS Code の左上にある変更タブの右端にある三点リーダーをクリックして「チェックアウト先...」を選択してください。

<img width="715" height="601" alt="スクリーンショット 2026-05-14 12 15 01" src="https://github.com/user-attachments/assets/dadac7dc-2bc3-40a9-aeb3-04acec93f1b2" />

新しいブランチの作成をクリックしてブランチ名を入力してください。

<img width="878" height="377" alt="スクリーンショット 2026-05-14 12 15 26" src="https://github.com/user-attachments/assets/85400c85-4606-4ead-87fc-28e072c07d7a" />

ブランチ名は `feature/問題番号` の形式にしてください（例: `feature/C001`）。

<img width="735" height="283" alt="スクリーンショット 2026-05-14 12 15 39" src="https://github.com/user-attachments/assets/a67912f6-8fd9-4e1a-84df-b9482e442b10" />


`main` ブランチから作成されていることを確認してください。

---

### 2-5. README.md に解法メモを書く

`README.md` に問題の解法や考え方があればメモしてください。
振り返りのときに役立ちます。

---

### 2-6. コミット・プッシュする

VS Code のソース管理パネルの変更タブの＋をクリックして `index.php` と `README.md` をステージングしてください。

<img width="411" height="231" alt="スクリーンショット 2026-05-14 12 31 36" src="https://github.com/user-attachments/assets/733fb05f-0f15-428f-b913-3598c477e099" />


コミットメッセージは **「問題番号: 解答にかかった時間」** の形式で入力し、コミットボタンをクリックしてください

```
C001: 9分55秒
```
<img width="401" height="353" alt="スクリーンショット 2026-05-14 12 16 29" src="https://github.com/user-attachments/assets/d4d56db9-e30c-4a83-b163-248411bbc1db" />


コミット後、「ブランチの発行」または「変更の同期」をクリックしてリモートにプッシュしてください。

<img width="411" height="277" alt="スクリーンショット 2026-05-14 12 22 17" src="https://github.com/user-attachments/assets/609c924e-ea5b-44b5-99d7-05901611c9c4" />


---

### 2-7. Pull Request を作成する

1. GitHub の Paiza リポジトリの「Pull requests」タブを開く
2. 「New pull request」をクリック
3. 作成した作業ブランチ（例: `feature/C001`）を選択する
4. 「Conversation」欄に **Paiza で問題を解いた結果画面のスクリーンショット** を貼り付ける
5. 画面右側の「Reviewers」で **神谷** を選択する
6. 「Create pull request」をクリックして PR を作成する

---

## チェックリスト

- [ ] C問題以上を 1 問解くことができる
- [ ] `feature/問題番号` ブランチを作成してコミット・プッシュできる
- [ ] Reviewers に神谷を設定して Pull Request を作成できる
