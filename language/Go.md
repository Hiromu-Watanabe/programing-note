# Go

<img src="../images/Go_image.png" width="100%" alt="Golangのイメージ">

[公式サイト : golang.org](golang.org)<br>
Go 言語は 2009 年に Google がリリースしたオープンソースのプログラミング言語。<br>
「シンプルで信頼性の高い効率的なソフトウェアを構築できる言語」と言われている。<br>
Go 言語は C 言語や Java などと同じく、静的型付け言語でありながら、Python や JavaScript などの動的型付け言語のような特徴を持つらしい。

<br><br>

### メリット

- 文法がシンプルである
- 高速な処理が可能
- 並列処理が得意
- メモリの安全性が高い

<br>

### デメリット

- 「継承」ができない
- 3 項演算子がない
- generics がない
- 例外処理ができない

<br><br>

## **<font color="#00ff00">関数宣言</font>**

Go では、名前が大文字で始まる関数は、別のパッケージの関数から呼び出すことができる（Javascript でいうところの Export してるのと同じ意味）<br>
[Exported names 参照](https://go.dev/tour/basics/3)

export されていない math パッケージの pi 関数（円周率を返却）

```Go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Println(math.pi)
}

// 出力 => ./prog.go:9:19: undefined: math.pi
```

export されている math パッケージの Pi 関数（円周率を返却）

```Go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Println(math.Pi)
}

// 出力 => 3.141592653589793
```

<br><br>

## **<font color="#00ff00">型</font>**

### 【map】

hash（連想配列）,オブジェクト指向言語におけるオブジェクトのようなもの。<br>

```go
// 基本構文
map[KeyType]ValueType // keyは比較可能な値ならば何でも良く, valueはどんな型でもOK

var m map[string]int // 文字列キーとint値のmap

// map型はポインターやスライスのような参照型のため上記のmはnilであり、初期化されたマップを指しているわけではない
// nillマップは読み込み時は空マップのように振る舞うが、nilマップに書き込もうとするとruntimeパニックを引き起こす


// 【runtimeパニックの例】
var m map[string]int // nilマップの作成
fmt.Println(m) // nilマップの読み込みは問題なく行える => 出力: "map[]"
m["key"] = 42 // しかし、nilマップに書き込もうとするとruntime panicが発生
// 実行 => エラーメッセージが表示されプログラムはクラッシュ
// エラーメッセージ => panic: assignment to entry in nil map


// マップの初期化には組み込みのmake関数を使う
m = make(map[string]int) // マップの初期化
fmt.Println(m) // 初期化されたマップの読み込みは問題なく行える => 出力: "map[]"
m["key"] = 42 // 初期化されたマップに"key"というキーに42を書き込むことも問題ない
fmt.PrintLn(m) // 出力: "map[key:42]"

m["route"] = 66 // key"route"に66を代入
i := m["route"] // 変数iにマップmのkey"route"に格納されている値を代入
j := m["root"] // 存在しないkey"root"を代入
fmt.PrintLn(j) // 出力: 0
// 要求されたキーが存在しない場合は、値型のゼロ値を取得
// この場合はマップmのvalueがintなのでゼロ値は0

これ以上の詳細は以下を参照
// https://go.dev/blog/maps
```

<br><br>

## **<font color="#00ff00">組み込み関数</font>**

JavaScript でいう map や sort, forearch みたいな組み込み関数。

### 【make】

特定の組み込み型（スライス、マップ、チャネル）の初期化とメモリ割り当てを行うために使用される。<br>
これらの型は参照型であり、make 関数を使用して初期化しないとゼロ値（nil）となる。

```go
1. スライスの作成
s := make([]int, 5) // 長さと容量が5のスライスを作成
fmt.Println(s)
// 出力 => [0 0 0 0 0]

2. マップの作成
m := make(map[string]int) // 空のマップを作成
fmt.Println(m)
// 出力 => map[]

3. チャネルの作成
c := make(chan int) // int型のデータを送受信するチャネルを作成
fmt.Println(c)
// 出力 => map[]
```

make 関数は、これらの型のメモリを確保し、適切に初期化するために必要。
