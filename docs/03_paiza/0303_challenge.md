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

<img width="533" height="294" alt="スクリーンショット 2026-05-15 10 34 05" src="https://github.com/user-attachments/assets/4e773d5c-6fcd-4cf0-8800-2ba529f92265" />



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

<img width="715" height="601" alt="スクリーンショット 2026-05-14 12 15 01" src="https://github.com/user-attachments/assets/ef6c3dd0-d969-4ea2-b3dd-5cd55644c365" />


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
C001: 5分20秒
```
<img width="401" height="353" alt="スクリーンショット 2026-05-14 12 16 29" src="https://github.com/user-attachments/assets/d4d56db9-e30c-4a83-b163-248411bbc1db" />


コミット後、「ブランチの発行」または「変更の同期」をクリックしてリモートにプッシュしてください。

<img width="411" height="277" alt="スクリーンショット 2026-05-14 12 22 17" src="https://github.com/user-attachments/assets/609c924e-ea5b-44b5-99d7-05901611c9c4" />


---

### 2-7. Pull Request を作成する

1. GitHub の Paiza リポジトリの「Pull requests」タブを開き、「New pull request」をクリック
   <img width="1419" height="634" alt="スクリーンショット 2026-05-15 10 05 13" src="https://github.com/user-attachments/assets/1bf5a70a-dee2-4575-b009-9c810a764191" />

2. 作成した作業ブランチ（例: `feature/C001`）を選択する
  <img width="1258" height="623" alt="スクリーンショット 2026-05-15 10 08 11" src="https://github.com/user-attachments/assets/6c8a4e9b-3af2-4a49-885c-ba85eed91895" />

3.ファイルの差分が問題なければ「Create pull request」をクリック
<img width="1401" height="618" alt="スクリーンショット 2026-05-15 10 36 49" src="https://github.com/user-attachments/assets/e01eb8fb-a220-4d7d-817b-759ee702d2b1" />

4. 「Conversation」欄に **回答した問題の問題文のスクリーンショット** を貼り付ける
5. 画面右側の「Reviewers」で **神谷** を選択する
6. 「Create pull request」をクリックして PR を作成する

<img width="1390" height="708" alt="スクリーンショット 2026-05-15 10 45 52" src="https://github.com/user-attachments/assets/26d75127-37c4-4ff1-92dc-b268d7c75930" />

---

## チェックリスト

- [ ] C問題以上を 1 問解くことができる
- [ ] `feature/問題番号` ブランチを作成してコミット・プッシュできる
- [ ] Reviewers に神谷を設定して Pull Request を作成できる
