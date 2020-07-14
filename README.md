# NACSIS-ILL サービスステータス集計
CiNii Books で提供されている、各参加館のサービスステータス（文献複写等の受付可否）を集計しています。
あくまで文献複写等の受付可否を表すもので、各館の休開館状況でありません。

## 集計方法
参加館データのうち、ILL参加種別、複写サービス種別、貸借サービス種別、サービスステータスのいずれかが N の場合、実質的にサービスを提供していません。このため、サービスを中止していると見なして集計しています。

{日付}.csv が取得したJSONファイルをまとめたものです。
都道府県ごとの集計は、Excelファイルをご確認ください。
テキストファイルの情報は以下の通りです。

|FA番号|KENCODE|都道府県名|CATFlag|ILLflag|CopyService|LoanService|FAXservice|ILLStatus|受付可否|館名|住所|
|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|:-|
|参加組織レコードID|県のコード|都道府県名|CAT参加種別|ILL参加種別|複写サービス種別|貸借サービス種別|FAXサービス種別|サービスステータス|ILLflag,CopyService,LoanService,ILLStatusのいずれかがNなら×|館名|住所|

### 各種別のフラグなど詳細
|フラグ|内容|
|:-|:-|
|A|可|
|N|否|
|C|他の窓口で受け付ける|

その他、詳細は [ILLシステム操作マニュアル第7版 8.2自館の参加組織レコードを更新する](http://catdoc.nii.ac.jp/MAN/ILL7/8_2.html)をご参照ください。

## 集計対象
* 2020年4月28日から作成しています。
* 元のデータは毎週月曜日未明に更新されます。このため、毎週日曜日時点での各館のサービスステータスを表します。
* 取得したデータは [library_status](library_status) 以下にあります。

## レシピ
1. CiNiiから参加館データ(例: https://ci.nii.ac.jp/library/FA001007.json )をJSON形式でダウンロード
2. jq を使用して必要なデータを抽出してcsvに出力。コマンドの例は以下

```cat *.json | jq .'"@graph"' | jq '[.[]."cinii:memberid", .[]."cinii:prefcode", .[]."cinii:catflag", .[]."cinii:illflag", .[]."cinii:copyservice", .[]."cinii:loanservice", .[]."cinii:faxservice", .[]."cinii:illstatus", .[]."v:fn",  .[]."v:adr"."v:label"] | @csv' > 20200518.csv```

3. 出力したファイルを集計

## データについて
* 集計は国立情報学研究所がCiNii BooksのAPIで提供する総合目録データベースのデータを使用しています。						
* ライセンスは元データと同じく クリエイティブ・コモンズ 表示 4.0 国際 ライセンス です。
  * 参考 総合目録データベースのデータ公開	https://www.nii.ac.jp/CAT-ILL/about/infocat/od/					
