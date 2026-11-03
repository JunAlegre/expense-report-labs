# measure-lab（Java のラボ）

ワークスペースのターミナルで、/home/labsuser/code に移動してから実行します。
番号はワークスペースの README.md の手順番号です。

## 1. ビルドする（ソースを変更したときだけ）

```
mvn install -Dmaven.test.failure.ignore=true
```

## 2. アプリを起動する

```
java -jar target/expense-report-api-1.0.0.jar --app.seed.reports=10 &
```

## 3. 一覧を取得する（Enter を1回押してから）

```
curl http://localhost:8080/api/reports; echo
```

## 4. アプリを止める

```
kill %1
```

## 5〜6. 計測する（件数を 10 → 100 → 500 と変えて3回）

```
java -jar target/expense-report-api-1.0.0.jar --app.benchmark.enabled=true --app.seed.reports=10 --spring.jpa.show-sql=false
```

```
java -jar target/expense-report-api-1.0.0.jar --app.benchmark.enabled=true --app.seed.reports=100 --spring.jpa.show-sql=false
```

```
java -jar target/expense-report-api-1.0.0.jar --app.benchmark.enabled=true --app.seed.reports=500 --spring.jpa.show-sql=false
```

## 7. 原因を探すファイル

```
src/main/java/com/example/expensereport/service/ExpenseReportService.java
```

メソッド: `listReports()`
