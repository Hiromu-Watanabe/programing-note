# AWS CLI とは

AWS のサービスを管理するための統合ツールです。 ダウンロードおよび設定用の単一のツールのみを使用して、コマンドラインから AWS の複数のサービスを制御し、スクリプトを使用してこれらを自動化することができる

<br><br>

### S3 オブジェクト一覧を取得

---

参考記事 : [S3 オブジェクトの一覧をあまり手間をかけずに AWS CLI で取得する](https://dev.classmethod.jp/articles/s3-objects-list-aws-cli/)

【特定のプレフィックス配下のオブジェクトのみを表示】
引数にプレフィックスを追加します。

```shell
$ aws s3 ls chibayuki-test-test/folderA --recursive --summarize --human-readable
2023-02-27 12:48:30    0 Bytes folderA/
2023-02-27 12:51:16    5 Bytes folderA/test_a.txt

Total Objects: 2
   Total Size: 5 Bytes
```

↑ ここでは chibayuki-test-test/folderA 配下のみを表示
