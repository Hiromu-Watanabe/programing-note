# **一般知識**

## **<font color="#00ff00">連想配列</font>**

一般的にハッシュ, ディクショナリ(辞書型)などとも呼ばれる。<br>
オブジェクト指向言語ではオブジェクトとも呼ばれる。

## **<font color="#00ff00">/helper フォルダ</font>**

- 特定のタスクを実行するための小さな関数が含まれることが多い
- 日付の書式設定、文字列の操作、配列の操作など

```js:実際の関数例
/* 日付を指定された書式で文字列に変換する関数。 */
formatDate(date: Date, format: string): string

/* 文字列を指定された最大長に切り詰める関数。 */
truncateString(str: string, maxLength: number): string

/* 配列内の数値の合計を計算する関数。 */
sumArray(arr: number[]): number
```

<br>
<br>

## **<font color="#00ff00">/common フォルダ</font>**

- プロジェクト全体で共有されるコンポーネントや定数、型定義などが含まれることが多い

```js:例
- Button.vue: プロジェクト全体で使用されるボタンコンポーネント。
- colors.ts: プロジェクト全体で使用される色の定数。
- types.d.ts: プロジェクト全体で使用される型定義。
```

<br>
<br>

## **<font color="#00ff00">開発手法</font>**

プロジェクトの進め方としていくつかの開発手法がある。

<br>
<br>

## **【アジャイル開発】**

アジャイル開発は、アプリやソフトウェア開発で用いられる手法の 1 つ。<br>
直訳すると「素早い」「機敏な」「頭の回転が速い」という意味がある。<br>

アジャイル開発では、納品するプロダクトまたはプロジェクトを[スプリント](https://www.atlassian.com/ja/agile/scrum/sprints)と呼ばれる小単位に区切って開発を進めるため、言葉の意味通り素早く、短い期間でシステムを開発することが可能です。

短いスパンで開発をするため、ビジネス要件に柔軟に対応できる。

<br>
<br>

| アジャイル開発の流れ |                                                                |
| :------------------- | :------------------------------------------------------------- |
| **`工程`**           | **`詳細`**                                                     |
| 1. 企画              | 「どんなシステムやソフトウェアを作りたいのか」を具体的に決める |
| 2. イテレーション    | 計画、設計、実装、テストのサイクルを回す                       |
| 3. リリース          | 完成したシステムをリリースする                                 |
| 4. 繰り返す          | 2〜3 の工程を繰り返す行う                                      |

<br>
<br>

## **【スクラム開発】**

[アジャイル開発の 1 種](https://codezine.jp/article/detail/12296)<br>

<br>
<br>

**【特徴】**

- 要求を価値やリスクや必要性を基準にして並べ替えて、その順にプロダクトを作ることで成果を最大化します

- スクラムでは固定の短い時間に区切って作業を進めます。固定の時間のことをタイムボックスと呼びます

- 現在の状況や問題点を常に明らかにします。これを透明性と呼びます

- 定期的に進捗状況や作っているプロダクトで期待されている成果を得られるのか、仕事の進め方に問題がないかどうかを確認します。これを検査と呼びます

- やり方に問題があったり、もっとうまくできる方法があったりすれば、やり方そのものを変えます。これを適応と呼びます

<br>
<br>

## **<font color="#00ff00">DB</font>**

<br>
<br>

## **【正規化】**

データベースの正規化は、データの冗長性を排除し、データの一貫性を保つためのプロセス

1. 第 1 正規形 (1NF): それぞれの属性値が原子的（分割不可能）であることを求めます。つまり、各セルには単一の値だけが含まれ、リストや配列のような複数の値を持つことは許されません。また、すべてのレコードはユニークなキーによって識別できるべきです。

<br>

2. 第 2 正規形 (2NF): 1NF に加えて、非キー属性がキーに完全に依存することを要求します。つまり、部分的な依存関係（キーの一部の属性のみに依存する非キー属性）が存在してはなりません。

<br>

3. 第 3 正規形 (3NF): 2NF に加えて、非キー属性間にトランジティブ（間接的）な依存関係が存在してはいけません。つまり、非キー属性が他の非キー属性に依存している場合、それらの間にトランジティブな依存関係が存在します。

<br>

4. ボイス・コッド正規形 (BCNF): 3NF に加えて、すべての決定的な依存関係が候補キーに対してのみ存在することを要求します。

<br>

5. 第 4 正規形 (4NF): BCNF に加えて、マルチバリュード依存性が存在しないことを要求します。

<br>

6. 第 5 正規形 (5NF) または 射影結合正規形 (PJNF): 4NF に加えて、結合依存性が存在しないことを要求します。

<br>
<br>

※ データベースをこれらの正規形に従って設計することにより、データの冗長性が削減され、データの一貫性と効率性が向上する。<br>
ただし、正規化が常に最良の選択とは限らず、パフォーマンスのために意図的に冗長性を残すという戦略もある。（登録のしやすさ、利用のしやすさや速度性能を考え、あえて冗長性を残す）

<br>
<br>

## **<font color="#00ff00">PC セットアップ</font>**

### 【GitHub 接続用 SSH キー作成】

- [SSH キー作成時のオプションなど](https://qiita.com/c_tomioka/items/379a9288d44634ca1daa)

- [新しい SSH キーを生成して ssh-agent に追加する](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [SSH キーのパスフレーズを使う](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases)
- [GitHub アカウントへの新しい SSH キーの追加](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

<br>
<br>

### 【anyenv】

[引用元記事](https://zenn.dev/hir0_728/scraps/8c5f622feb6c46)

anyenv を使って複数の\*\*env (nodenv, rbenv, pyenv など) を一括管理できるようにする。
homebrew 導入済みなので、セットアップは簡単。

公式のリポジトリは[こちら](https://github.com/anyenv/anyenv?tab=readme-ov-file)

```shell
# anyenv のインストール実施
$ brew install anyenv
$ anyenv init --init

$ vi ~/.zshrc

# 以下内容を入力する
# >> anyenv >>

alias brew="env PATH=${PATH/\/Users\/${USER}\/\.anyenv\/envs\/pyenv\/shims:/} brew"
eval "$(anyenv init -)"

# << anyenv <<

# wq:

$ exec $SHELL -l
$ anyenv -v

# \*\*env 環境を一括でアップデートできるプラグイン

$ mkdir -p $(anyenv root)/plugins
$ git clone https://github.com/znz/anyenv-update.git $(anyenv root)/plugins/anyenv-update
$ anyenv update

$ git clone https://github.com/znz/anyenv-git.git $(anyenv root)/plugins/anyenv-git


# インストール可能なenvの確認
$ anyenv install -l

# pyenvのインストールの例
$ anyenv install pyenv
$ exec $SHELL -l
$ pyenv -v
```

<br><br>

### 【nodeenv】

- https://retval.jp/blog/m1-mac-nodenv/#nodenv

### 【情報収集のサービス】

Artifact, Perplexity, Lenny's Newsletter

<br><br>

## **<font color="#00ff00">開発に関する用語など</font>**

### 【ポストモーテム】

何がうまくいき、うまくいかなかったかを特定し、組織的プロセスを変更することにより学んだ教訓を取り入れ、プロジェクトの改善をサポーとするプロセス。
障害があった際
