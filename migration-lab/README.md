# migration-lab（AWS のラボ）

## 始める前に

このラボの制限時間は60分です。時間切れになると作成したリソースは消え、続きからは再開できません。
時間内に終わらなくても構いません。答え合わせ用の動画を見て流れをつかめば、やり直しは何度でもできます。
リージョンは米国東部（バージニア北部）us-east-1 のまま操作してください。

番号は課題の番号です。

## 課題1: DynamoDB のテーブル

| 項目 | 値 |
|---|---|
| テーブル名 | `ExpenseReports` |
| パーティションキー | `reportId`（文字列） |

## 課題2: Lambda 関数

| 項目 | 値 |
|---|---|
| ランタイム | Java 21 |
| ハンドラ | `com.example.expensereport.lambda.ExpenseReportLambdaHandler::handleRequest` |
| 環境変数のキー | `TABLE_NAME` |
| 環境変数の値 | `ExpenseReports` |
| タイムアウト | 30秒 |

jar は課題2のリソースからダウンロードします。

## 課題3: API Gateway（HTTP API）のルート

| 項目 | 値 |
|---|---|
| メソッド | `ANY` |
| リソースパス | `/{proxy+}` |

エンドポイントの末尾に付けるパス:

```
/api/reports
```

## 課題4: ログの検索語

```
not authorized
```

## 課題5: 実行ロールの権限

ポリシー名:

```
ExpenseReportsLabAccess
```

インラインポリシーの JSON:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["dynamodb:Scan", "dynamodb:GetItem", "dynamodb:PutItem"],
      "Resource": "arn:aws:dynamodb:us-east-1:*:table/ExpenseReports"
    }
  ]
}
```

## 課題6: データ投入

一時的に変えるハンドラ:

```
com.example.expensereport.lambda.SeedLambdaHandler::handleRequest
```

テストイベント:

```json
{"reports": 25}
```

終わったらハンドラを課題2の値に戻します。

## 課題7: CloudWatch で見るメトリクス

| 項目 | 値 |
|---|---|
| メトリクス | `SuccessfulRequestLatency`（Operation が `Scan`） |
| 統計 | サンプル数 |
| 期間 | 1分 |
