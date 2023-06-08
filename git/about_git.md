# Git について

<img src="../images/Git_image.png" width="100%" alt="Gitのイメージ">

<br />
<br />

## **<font color="#00ff00">Git config （設定情報）</font>**

---

[この記事](https://note.nkmk.me/git-config-setting/)が参考になった。

<br />

※ Git の設定ファイルは 3 種類
| 種類 | 対象範囲 | 場所の例 | 備考 |
| :-: | :- | :- | :- |
| system | システム全体（全ユーザーの全リポジトリ） | `etc/gitconfig` | - |
| global | 該当ユーザーの全リポジトリ | `~/.gitconfig` | ホーム直下 |
| local | 該当リポジトリ | `repository/.git/config` | リポジトリの`.git`直下 |

<br />
<br />

---

**【リポジトリごとに個別に Git のユーザーを設定する方法】**

1. 設定したいリポジトリのパスまで移動

2. 一応リポジトリの Git の設定情報を確認

```
$ git config --local -l
```

3. 設定情報を修正
   user.name の修正

```
$ git config user.name "{設定したい名前}"
```

user.email の修正

```
$ git config user.email "{設定したいメアド}"
```

<br />
<br />

---

**【コミットメッセージ】**

[この記事](https://qiita.com/itosho/items/9565c6ad2ffc24c09364)を参考にしてる。

これが正解ではないので、プロジェクトや開発しながらこうした方が良いと思ったものは取り入れて<br />
進化させていくべき。

<br />

**通常盤**

- fix：バグ修正
- hotfix：クリティカルなバグ修正
- add：新規（ファイル）機能追加
- update：機能修正（バグではない）
- change：仕様変更
- clean：整理（リファクタリング等）
- disable：無効化（コメントアウト等）
- remove：削除（ファイル）
- upgrade：バージョンアップ
- revert：変更取り消し

<br />

**ライト版**

- fix：バグ修正
- add：新規（ファイル）機能追加
- update：機能修正（バグではない）
- remove：削除（ファイル）
