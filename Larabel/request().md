# request()

`request()`は、HTTPリクエストに関連する情報を取得するためのヘルパー関数。

## 様々な種類

```php
// リクエストデータの取得
$name = request()->input('name'); // "name"の値を取得
$email = request('email');       // ショートハンド形式
$page = request()->input('page', 1); // 第二引数でデフォルト値を設定可能、この例では値がない場合は1を返す


//リクエストバリデーション
$data = request()->validate([
    'name' => 'required|string|max:255',
    'email' => 'required|email',
]);


// リクエストメソッドの判定
if (request()->isMethod('post')) {
    // POSTリクエスト専用の処理
}


// クエリパラメータの取得
// GETリクエストでの検索条件やページネーションなどに使用
$page = request()->query('page', 1); // クエリパラメータ "page" を取得


// リクエストパスやURLの判定
// ルーティングの条件分岐やアクセス制限で使用
if (request()->is('admin/*')) {
    // URLが "admin/" で始まる場合の処理
}


// ファイルのアップロード
if (request()->hasFile('photo') && request()->file('photo')->isValid()) {
    $path = request()->file('photo')->store('uploads');
}
// hasFile()でファイルの有無を確認。
// isValid()でファイルの妥当性をチェック。


// JSONデータの取得
$jsonData = request()->json()->all(); // JSON形式の全データを取得


// リクエストヘッダーの取得
// 認証やカスタムヘッダーの確認で使用
$token = request()->header('Authorization');


// リクエスト全体の取得
$allData = request()->all(); // 全入力データを配列で取得


// リファラ情報の取得
$referrer = request()->headers->get('referer');
```