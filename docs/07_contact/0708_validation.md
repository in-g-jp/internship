# 0403 ログイン - バックエンドバリデーション

## 1. FormRequest の作成

```bash
sail artisan make:request LoginRequest
```

---

## 2. バリデーションルールの実装

`app/Http/Requests/LoginRequest.php` を編集してください。

```php
public function authorize(): bool
{
    return true;
}

public function rules(): array
{
    return [
        'email'    => ['required', 'email'],
        'password' => ['required', 'string'],
    ];
}

public function messages(): array
{
    return [
        'email.required'    => 'メールアドレスは必須です。',
        'email.email'       => 'メールアドレスの形式が正しくありません。',
        'password.required' => 'パスワードは必須です。',
    ];
}
```

---

## チェックリスト

- [ ] 空のリクエストを送ると 422 エラーが返る
- [ ] メールアドレスの形式が不正なとき 422 エラーが返る

---

## 学習

### 今何をしたか

ログイン API のバリデーションを実装しました。
ログインの失敗理由（「メールが違う」「パスワードが違う」）は、セキュリティ上あえて区別せず同じメッセージを返すのが一般的です。

| 用語 | 説明 |
|------|------|
| ユーザー列挙攻撃 | 「このメールは存在しません」というエラーを使って有効なアカウントを調べる攻撃手法 |

### 参考資料

- [Laravel バリデーション](https://readouble.com/laravel/12.x/ja/validation.html)
