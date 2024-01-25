## ターミナル

入出力の画面を提供するソフトウェア。（画面の担当）<br>
実際にユーザーがコマンドを入力する画面。<br>
ターミナルの中でシェルが動いている。

<br>
<br>

### ターミナルの長ったらしい表示を短くする設定

【参考記事一覧】

- https://qiita.com/tonkotsuboy_com/items/b752a86cee7eaedf28da
- https://syslog.life/2020/10/16/linux-mac-%E3%82%BF%E3%83%BC%E3%83%9F%E3%83%8A%E3%83%AB-%E8%A1%A8%E7%A4%BA%E8%A8%AD%E5%AE%9A-%E3%83%9B%E3%82%B9%E3%83%88%E5%90%8D-%E9%9D%9E%E8%A1%A8%E7%A4%BA/
- https://tech.qookie.jp/posts/terminal-turn-off-prompt/
- https://reasonable-code.com/change-prompt/
- https://eng.shibuya24.info/entry/zsh_remove_pcname_and_username

<br>
<br>

---

## shell

ユーザーと Linux の仲介役。（コマンドを解釈して Linux とやり取りする担当）<br>
シェルは Linux のコマンドを受け取って結果を出力するためのソフトウェア。<br>
bash もシェルの一種。

```text: Linuxのコマンド実行の流れ
→ ユーザーがコマンドを入力
→ シェルが命令を解釈してLinuxに伝える
→ Linuxがコマンド実行
→ シェルが実行結果をユーザーに伝える
```
