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

<br><br>

## **<font color="#00ff00">テスト</font>**

Go には組み込みでユニットテストがサポートされている。<br>
ファイル名の最後を<font color="orange">_test.go_</font>とすることで、「このファイルにはテスト関数が含まれてんで」と伝える。

### 【テストの例】

テスト対象のファイル

```go
// ファイル名: gteetings.go

package greetings

import (
	"errors"
	"fmt"
	"math/rand"
)

// Hello returns a greeting for the named person.
func Hello(name string) (string, error) {
	// If no name was given, return an error with a message.
	if name == "" {
		return "", errors.New("empty name")
	}

	// Create a message using a random format.
	message := fmt.Sprintf(randomFormat(), name) // テスト成功パターン
	message := fmt.Sprint(randomFormat()) // テスト失敗パターン
	return message, nil
}

// Hellos returns a map that associates each of the named people
// with a greeting message.
func Hellos(names []string) (map[string]string, error) {
	// A map to associate names with messages.
	messages := make(map[string]string)
	// Loop through the received slice of names, calling
	// the Hello function to get a message for each name.
	for _, name := range names {
			message, err := Hello(name)
			if err != nil {
					return nil, err
			}
			// In the map, associate the retrieved message with
			// the name.
			messages[name] = message
	}
	return messages, nil
}

// randomFormat returns one of a set of greeting messages. The returned
// message is selected at random.
func randomFormat() string {
	// A slice of message formats.
	formats := []string{
		"Hi, %v. Welcome!",
		"Great to see you, %v!",
		"Hail, %v! Well met!",
	}

	// Return a randomly selected message format by specifying
  // a random index for the slice of formats.
	return formats[rand.Intn(len(formats))]
}
```

テストファイル

```go
// ファイル名: greetings_test.go

package greetings

import (
    "testing"
    "regexp"
)

// TestHelloName 名前を指定してgreetings.Helloを呼び出し、
// 有効な戻り値があるかどうかをチェックするテスト関数
func TestHelloName(t *testing.T) {
    name := "Gladys"
    want := regexp.MustCompile(`\b`+name+`\b`)
    msg, err := Hello("Gladys")
    if !want.MatchString(msg) || err != nil {
        t.Fatalf(`Hello("Gladys") = %q, %v, want match for %#q, nil`, msg, err, want)
    }
}

// TestHelloEmptyは空の文字列でgreetings.Helloを呼び出す
// エラーをチェックするテスト関数.
func TestHelloEmpty(t *testing.T) {
    msg, err := Hello("")
    if msg != "" || err == nil {
        t.Fatalf(`Hello("") = %q, %v, want "", error`, msg, err)
    }
}
```

### 【テストを実行】

テスト成功パターン

```shell
# go testコマンドは、テストファイル（名前が_test.goで終わる）内のテスト関数（名前がTestで始まる）を実行する
$ go test
# [出力]
# PASS
# ok      {モジュール名}  0.002s

# vフラグを追加すると、すべてのテストとその結果を一覧表示することができる。
$ go test -v
# [出力]
# === RUN   TestHelloName
# --- PASS: TestHelloName (0.00s)
# === RUN   TestHelloEmpty
# --- PASS: TestHelloEmpty (0.00s)
# PASS
# ok      {モジュール名}  0.003s
```

テスト失敗パターン

```shell
# -v フラグを付けずに go test を実行
#  失敗したテストだけが出力されるので、テストがたくさんある場合には -v フラグはない方が良さそう
$ go test
# [出力]
# --- FAIL: TestHelloName (0.00s)
#     greetings_test.go:15: Hello("Gladys") = "Great to see you, %v!", <nil>, want match for `\bGladys\b`, nil
# FAIL
# exit status 1
# FAIL    {モジュール名}  0.003s

TestHelloNameテストは失敗して、TestHelloEmptyはまだパスしている。（正しくテスト動作している）


# -v フラグ付けたバージョン
$ go test -v
# [出力]
# === RUN   TestHelloName
#     greetings_test.go:15: Hello("Gladys") = "Great to see you, %v!", <nil>, want match for `\bGladys\b`, nil
# --- FAIL: TestHelloName (0.00s)
# === RUN   TestHelloEmpty
# --- PASS: TestHelloEmpty (0.00s)
# FAIL
# exit status 1
# FAIL    github.com/Hiromu-Watanabe/go-tutorial  0.003s
```
