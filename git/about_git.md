# Git について

<img src="../images/Git_image.png" width="100%" alt="Gitのイメージ">

<br />
<br />

## **<font color="#00ff00">Git の仕組み図</font>**

<img src="../images/Git_structure_image.png" width="100%" alt="Gitのイメージ">

<br />
<br />

---

## **<font color="#00ff00">Git config （設定情報）</font>**

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

## **<font color="#00ff00">リモート設定</font>**

現在のリモート設定を確認

```shell
$ git remote -v

# 出力
grc     codecommit::ap-northeast-1://{リポジトリ名} (fetch)
grc     codecommit::ap-northeast-1://{リポジトリ名} (push)
origin  git@github.com:{ユーザー or 組織名}/{リポジトリ名}.git (fetch)
origin  git@github.com:{ユーザー or 組織名}/{リポジトリ名}.git (push)
```

<br>

origin の URL を設定

```shell
$ git remote set-url origin [GitHubのリポジトリのURL]
```

<br>

今いるブランチ`pull`コマンド実行時のデフォルトリモートを設定

```shell
# 例)`origin/stg`をpullコマンドのデフォルトのリモートに設定したい場合
$ git branch --set-upstream-to=origin/stg stg

# 出力
Branch 'stg' set up to track remote branch 'stg' from 'origin'.
```

<br />
<br />

---

## **<font color="#00ff00">リポジトリ作成後の初っ端</font>**

**【リポジトリをそのまま clone】**

```shell
$ git clone https://github.com/${ユーザー名}/${リポジトリ名}.git
```

**【その他もろもろパターン】**

<img src="../images/start_repository.png" width="100%" alt="GitHubから拝借">

<br />
<br />

---

## **<font color="#00ff00">コミットメッセージについて</font>**

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

<br />
<br />

---

## **<font color="#00ff00">stash</font>**

**1. 変更を退避**

```shell: スタッシュのコマンド
$ git stash # スタッシュのメッセージを設定せずにスタッシュする
$ git stash save "message" # スタッシュのメッセージを設定してスタッシュする
$ git stash -u # 未追跡のファイルも全てスタッシュされる
$ git stash -k # `git stash --keep-index` : 追跡されているファイルをスタッシュに保存するが、ステージングエリアにある変更はそのまま保持

# .gitignoreのファイルは依然としてスタッシュに含まれない
```

コミットしていない変更がある状態で上記のコマンドを実行すると、コミットしていない変更した部分が退避される。<br />
「コミットしていない変更」とは、`git addしたもの`も`git addしていないもの`もどちらも含まれる。

<br />

**2. 退避した作業の一覧を見る**

```shell: 退避した内容の一覧を見るコマンド
$ git stash list
```

<br />

**3. pull などする**

<br />

**4. 指定した退避内容を戻す**

```shell: 退避した内容を戻すコマンド
$ git stash apply stash@{[スタッシュ番号]}
```

<br />
<br />

**5. 指定した退避内容を削除する**

```shell: 退避した内容を削除コマンド
$ git stash drop stash@{N} # N番目のスタッシュを削除
$ git stash clear # スタッシュを全削除
```

<br />
<br />

**【番外編】指定した退避内容を戻すと同時に削除する**

```shell: 指定した退避内容を戻すと同時に削除するコマンド
$ git stash pop # 最新のスタッシュを適用し、削除
$ git stash pop stash@{n} # N番目のスタッシュを適用し、削除
```

<br />
<br />

**【Tips】**

N 番目にスタッシュしたファイルの一覧を表示

```shell
$ git stash show stash@{N}
```

<br />

N 番目にスタッシュしたファイルの変更差分を表示

```shell
$ git stash show -p stash@{N}
```

<br />

N 番目にスタッシュしたファイルの変更差分を表示

```shell
$ git stash show -p stash@{N}
```

<br />
<br />

---

## **<font color="#00ff00">ブランチ</font>**

**1. ブランチの一覧を表示**

ローカルブランチの一覧

```shell
$ git branch
```

すべてのブランチ（リモートにある他の人が切ったブランチも含む）

```shell
$ git branch -a
```

**2. ブランチの追加**

```shell
$ git branch {追加したいブランチ名}
```

**3. ブランチの切り替え**

```shell
# ブランチの一覧を確認
$ git branch
## 出力 ##
# * develop
#   stg
#   prod
#   main

$ git checkout stg
## 出力 ##
#   develop
# * stg
#   prod
#   main
```

**3.5. ブランチの作成 & 切り替え**

```shell
git checkout -b {ブランチ名}
```

**4. ブランチの削除**

```shell
$ git branch -d {削除したいブランチ名}
```

<br />
<br />

---

## **<font color="#00ff00">コミットに関するコマンド</font>**

**コミット取り消し**

```shell
$ git reset [打ち消したいコミットID]

# 【resetオプション】

# --softオプション
$ git reset --soft
# commitのみ取り消し（HEADの位置のみ修正）
# 「コミットをする直前」の状態に戻る
# 作業ディレクトリとステージングエリアはそのまま
# 例）まとめてコミットしたかったのに1つしかコミットしていなかったとき
# 例）まだ作業中なのにコミットしてしまったとき

# -- mixedオプション
$ git reset --mixed
# commitとaddの取り消し（HEADの位置・インデックスが修正）
# HEADと一緒にステージが巻き戻る
# 作業ディレクトリのファイルは消えない
# 例）git addでステージしたけど、やっぱり戻したいとき

# -- hardオプション
$ git reset --hard
# 全部を取り消し（HEADの位置・インデックス・ワーキングツリーが修正）
# もっとも強力なオプション
# 例）ステージングエリアにも作業ディレクトリにも残したくないといった
```

<br />
<br />

---

## **<font color="#00ff00">マージに関するコマンド</font>**

マージの実行（main ブランチに dev ブランチをマージ）

```shell
# マージを実行したいブランチに移動
$ git checkout main

# mainブランチにdevブランチをマージ実行
$ git merge dev
```

マージ操作の中止

```shell
$ git merge --abort
```
