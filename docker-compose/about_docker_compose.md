[ムーザルちゃんねるの youtube](https://www.youtube.com/watch?v=M3h_z-9DoMQ)が参考になった<br>
[youtube 内の資料](https://speakerdeck.com/mu_zaru/sakututoli-tishang-garuhuan-jing-gou-zhu-docker-compose-ru-men)

<br>

現在は docker-compose の後発として Docker のサブコマンドで`compose`オプションがある。<br>
互換性を保ち docker-compose と同じ機能が使える。
docker-compose コマンドの将来的な置き換えを目指して開発された機能のため、`compose`オプションを使用した方が良さそう？

<br>

# docker-compose とは

複数コンテナを立てる場合に、Docer だけで管理するのは「個別の起動」や「ネットワークの指定」がめんどくさい<br>
それらを docker-compose コマンド（1 コマンド）でいい感じに管理とか起動ができる。<br>
Docker とは別にインストールが必要なはず。<br>
ただ、Docker for Mac とかをインストールすると自動で入っている

<br>

docker-compose を使う理由は、Docker だけだと「アプリのコンテナ」 「DB のコンテナ」 「web サーバーのコンテナ」の 3 つのコンテナがある場合に

## **【Docker のみ】**

アプリのコンテナを立ち上げる

```bash: appコンテナ立ち上げ
$ docker run {イメージ名}
```

<br>

DB のコンテナを立ち上げる

```bash: DBコンテナ立ち上げ
$ docker run {イメージ名}
```

<br>

web サーバーのコンテナを立ち上げる

```bash: webサーバーコンテナ立ち上げ
$ docker run {イメージ名}
```

<br><br>

## **【docker-compose】**

docker-compose.yml があるディレクトリで以下コマンドを叩くと yml ファイル内にそれぞれに設定した内容でコンテナが立ち上がる。<br>
複数ではなく、1 つだけのコンテナの場合も docker-compose を使った方が便利らしい？

```bash: docker-composeの場合
$ docker-compose up
```

### docker-compose.yml の例

<img src="../images/docker-compose.yml_example.png" width="100%" alt="docker-compose.ymlの例">

※ [上記画像の引用元](https://speakerdeck.com/mu_zaru/sakututoli-tishang-garuhuan-jing-gou-zhu-docker-compose-ru-men?slide=11)

- volumes しかり ports しかり 左がホスト（mac, windows などの自分が触ってるマシン）で:(コロン)の右側がコンテナ
- 上記画像の例では image に Docker Hub に公開されているイメージ名を指定している
- 実際のアプリ開発でアプリケーションコンテナ（Next.js, Ruby on Rails）は自前の Dockerfile でイメージを作成
- MySQL, Redis, nginx などのミドルウェアは Docker Hub に公開されている公式のイメージをそのまま使うことが多いらしい

<br><br>

---

## 【docker-compose でのコンテナ起動】

docker-compose.yml ファイルを作ったディレクトリで以下コマンド実行

```bash: コンテナ起動コマンド
$ docker-compose up -d
```

※ "-d"オプションはバックグラウンドでコンテナを起動してくれる<br>
（"-d"オプションがないとターミナルが起動したコンテナで占領されて$ Ctr + c で抜けるまでターミナル触れない）

<br><br>

例）go-tutorial というプロジェクト(ディレクトリ)配下で Golang のコンテナを以下の`Dockerfile`と`docker-compose.yml`で作成した場合、「**go-tutorial-app-1**」というコンテナが作成される。<br>
この末尾の「-1」は`docker-compose up`を実行すると、デフォルトでは、コンテナ名は

```
<project>_<service>_<index>
```

という形式で作成される。

\<project> : ディレクトリ名<br>
\<service> : サービス名<br>
\<index> : 同じサービスから生成されたコンテナのインスタンス番号（1 つ目のコンテナには "-1" 、2 つ目には "-2"）

<br>

【Dockerfile】

```Dockerfile
version: "3" # composeファイルのバージョン
services:
  app: # サービス名
    build: . # ビルドに使用するDockerfileの場所
    tty: true # コンテナの永続化
    volumes:
      - ./app:/go/src/app # マウントディレクトリ
```

【docker-compose.yml】

```yml
version: "3" # composeファイルのバージョン
services:
  app: # サービス名
    build: . # ビルドに使用するDockerfileの場所
    tty: true # コンテナの永続化
    volumes:
      - ./app:/go/src/app # マウントディレクトリ
```

<br><br>

【コンテナ名に index を付けさせないようにする方法】

「container-name: 」を使用してコンテナに固定の名前を指定することができる。

```yml
version: "3"
services:
  app:
    build: .
    tty: true
    volumes:
      - ./app:/go/src/app
    container_name: go-tutorial-app # この部分でコンテナ名を固定させる
```

※ ただし、container_name を使用すると同一サービス（今回で言えば app）から複数のコンテナを作成することができない。例えば次のようなケース

```yml
version: "3"
services:
  app:
    build: .
    tty: true
    volumes:
      - ./app:/go/src/app
    container_name: go-tutorial-app
    scale: 2 # scaleによってappサービスから2つのコンテナを作成しようとしている
```

`container_name:`で固定の名前を割り当ててしまうと、2 つ目のコンテナを作成できなくなる。<br>
⏩ Docker のコンテナ名はユニークでなければならないから

<br>

`container_name:`は、そのサービスから一つだけのコンテナを作成する場合、または明示的にそのコンテナに固定の名前を割り当てたい場合にのみ使用します。複数のインスタンスを作成したい場合や、Docker Compose によるデフォルトの命名規則で問題ない場合は、`container_name:`は省略した方が良さげ。

<br><br>

---

## 【docker-compose でのコンテナに入る】

docker-compose.yml ファイルを作ったディレクトリで以下コマンド実行

```bash: コンテナに入るコマンド
$ docker-compose exec {コンテナ名} bash
```

bash を指定するとコンテナ内部の bash でコンテナ内に入ることができる。<br>
このコマンドはお決まりの呪文みたいなものなので、覚えてしまう。

※ Linux のコマンド書くとコンテナに対して Linux コマンドを実行できる

<br><br>

# docker compose オプション

- コンテナ作成(ビルド)

```shell
$ docker compose up -d --build ${コンテナ名}
```
