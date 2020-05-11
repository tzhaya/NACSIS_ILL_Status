# NACSIS-ILL サービスステータス集計
CiNii Books で提供されている、各参加館のサービスステータス（文献複写等の受付可否）を集計しています。

## 集計方法
参加館データのうち、ILL参加種別、複写サービス種別、貸借サービス種別、サービスステータスのいずれかが N の場合、サービスを中止していると見なして集計しています。

集計している情報は以下の通りです。

|FA番号|KENCODE|都道府県名|CATFlag|ILLflag|CopyService|LoanService|FAXservice|ILLStatus|受付可否|館名|住所|
|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|
|館の番号|県のコード|都道府県名|CAT参加種別|ILL参加種別|複写サービス種別|貸借サービス種別|FAXサービス種別|サービスステータス|ILLflag,CopyService,LoanService,ILLStatusのいずれかがNなら×|館名|住所|

## レシピ
1. CiNiiから参加館データ(例: https://ci.nii.ac.jp/library/FA001007.json)をJSON形式でダウンロード
2. jq を使用して必要なデータを抽出してcsvに出力。コマンドの例は以下

```cat *.json | jq .'"@graph"' | jq '[.[]."cinii:memberid", .[]."cinii:prefcode", .[]."cinii:catflag", .[]."cinii:illflag", .[]."cinii:copyservice", .[]."cinii:loanservice", .[]."cinii:faxservice", .[]."cinii:illstatus", .[]."v:fn",  .[]."v:adr"."v:label"] | @csv' > list.csv```

3. 出力したファイルを集計
