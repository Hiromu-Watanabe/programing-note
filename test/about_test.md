# テストについて

テストコードを書くことで安全な開発が行える

Javascript の一般的なテストフレームワークは Jest?

※[サバイバル TypeScript に Jest の例があった](https://typescriptbook.jp/tutorials/jest)

**【メリット】**

1.  バグの早期発見

        テストコードを書くことにより、コードの挙動を確認し、バグを早期に発見できます。例えば、特定の入力に対する関数の出力を検証することで、予期しない挙動を検出できます。

    <br/>
    <br/>

2.  リファクタリングの容易さ

        テストコードがあると、コードのリファクタリングが安全に行えます。既存のテストがパスする限り、機能が保持されていると確認できます。

    <br/>
    <br/>

3.  コードの品質向上

        Jest は TypeScript をサポートしているため、静的型チェックと単体テストによりコードの品質が向上します。また、Vue3 の Composition API をテストすることで、リアクティブなデータの流れを理解しやすくなります。

    <br/>
    <br/>

4.  ドキュメンテーション

        テストコードはソフトウェアのドキュメンテーションとしても機能します。新たにプロジェクトに参加した開発者がテストコードを読むことで、各関数がどのように動作するかを理解するのに役立ちます。

    <br/>
    <br/>

**【デメリット】**

1.  開発時間の増加

        テストコードを書くためには、その分の時間が必要になります。短期的には開発速度が低下する可能性があります。

   <br/>
   <br/>

2.  適切なテスト設計の難易度

         テストコードを書く際には、どの部分をテストするか、どのようにテストするかといったテスト設計についても深く考える必要があります。これは容易な作業ではなく、特に経験やスキルがない場合は難しいかもしれません。

    <br/>
    <br/>

3.  テストの保守コスト

         コードベースが進化するにつれて、テストコードもそれに追従して更新する必要があります。テストが古くなったり、関連性を失ったりした場合にはそれらを削除または更新する必要があります。

    <br/>
    <br/>

4.  フレームワークやライブラリの変更に対する脆弱性

         Jest や Vue3 などのフレームワークやライブラリが更新されたとき、互換性の問題が生じる可能性があります。これによりテストコードの修正が必要になる場合があります。

    <br/>
    <br/>

メリット・デメリットを考慮に入れてプロジェクトにおけるテストコードの<font color="Red">利点</font>と<font color="Red">コスト</font>をバランスよく評価することが大事

<br>
<br>

# **<font color="#00ff00">Vitest</font>**

<img src="../images/Vitest_image.png" width="100%" alt="Vue.jsのイメージ">

Vitest は、Vite ベースのテスティングフレームワークです[^1]。

Vitest は、公式で「Blazing Fast Unit Test Framework」と謳われています[^1]。つまり、とても高速なユニットテストフレームワークです。

Vitest には、いくつかの特徴があります。Vite の config、transformers、resolvers と plugins をアプリと同じ設定で使用できます。また、Vite の HMR のような watch モードがあり、Vue、React、Svelte、Lit などのコンポーネントテストをサポートしています。さらに、TypeScript や JSX がサポートされており、ESM ファーストで top level await が可能です[^1]。

Jest との関係性については、Jest は JavaScript、TypeScript におけるテストツールとして成熟したツールですが、Vite 環境においては Vitest の方が使い勝手が良く積極的に利用を始めても良いとされています[^1]。また、Vitest は Jest 互換のスナップショットを持ちます[^1]。

以上が私の知っている情報です。他に何か知りたいことはありますか？

[^1]: https://qiita.com/KokiSakano/items/f23e979004f72d70165e.

**【その他記事】**

[Vitest と呼ばれるテスティングフレームワークがめちゃくちゃ....](https://azukiazusa.dev/blog/testingframework-vitest/.)

[【Vitest】絶賛開発中の Vitest を使ってみる - Qiita.](https://qiita.com/tak001/items/3491758d95bfee2c65bc.)

[Vitest を使って Unit Testing にチャレンジ(Vue, React, Svelte).](https://reffect.co.jp/html/vitest.)

[あたらしいテストフレームワーク Vitest を React で試してみた ....](https://dev.classmethod.jp/articles/intro-vitest/.)

**【使用例】**

以下の関数をテストする場合

<font color="orange">UnitPriceUtils.ts</font>

```ts: テスト対象のコード
/**
 * 消区(消費税区分)： "別"(税抜)の場合
 * @param {string} taxationCategory -[商品].[課税区分]
 * @param {number} unitPrice -[商品].[本体価格] or [価格改定].[単価]
 * @param {number | null | undefined} standardTaxRate -通常の税率
 * @param {number | null | undefined} standardTaxRate -軽減された税率
 * @param {string} screenName -画面名
 * @returns {number} 計算後の商品単価
 */
export const calculateTaxExcludedPrice = async (
  taxationCategory: string,
  unitPrice: number,
  standardTaxRate: number | null | undefined,
  reducedTaxRate: number | null | undefined,
  screenName: string
): Promise<number> => {

  let taxRatePermille: number

  switch (taxationCategory) {
    /* 課税区分 === "通常商品" */
    case TaxationCategory.Regular:
      if (!standardTaxRate) {
        throw new Error(`【${screenName}】通常の税率が取得できません`)
      }
      taxRatePermille = standardTaxRate * 10
      return Math.floor((unitPrice * (1000 + taxRatePermille)) / 1000)
    /* 課税区分 === "軽減税率対象商品" */
    case TaxationCategory.ReducedTaxRate:
      if (!reducedTaxRate) {
        throw new Error(`【${screenName}】軽減された税率が取得できません`)
      }
      taxRatePermille = reducedTaxRate * 10
      return Math.floor((unitPrice * (1000 + taxRatePermille)) / 1000)
    /* 課税区分 === "経過措置対象商品" */
    case TaxationCategory.TransitionalMeasures:
      // TODO: 経過措置商品の場合の単価計算処理
      return 0
    default:
      throw new Error(`【${screenName}】課税区分が不明です`)
  }
}
```

以下のようなテストコードになる

<font color="orange">UnitPriceUtils.test.ts</font>

```ts: vitestのテストコード例
import { describe, expect, test } from 'vitest'
import { calculateTaxExcludedPrice } from './UnitPriceUtils'
import { TaxationCategory } from '../helper/const'

describe('calculateTaxExcludedPrice function', () => {
  test('should correctly calculate for Regular products', async () => {
    const result = await calculateTaxExcludedPrice(
      TaxationCategory.Regular,
      1000,
      8,
      null,
      'TestScreen'
    )
    expect(result).toEqual(1080)
  })

  test('should throw an error if standard tax rate is null for Regular products', async () => {
    await expect(
      calculateTaxExcludedPrice(
        TaxationCategory.Regular,
        1000,
        null,
        null,
        'TestScreen'
      )
    ).rejects.toThrow('通常の税率が取得できません')
  })

  test('should correctly calculate for ReducedTaxRate products', async () => {
    const result = await calculateTaxExcludedPrice(
      TaxationCategory.ReducedTaxRate,
      1000,
      null,
      5,
      'TestScreen'
    )
    expect(result).toEqual(1050)
  })

  test('should throw an error if reduced tax rate is null for ReducedTaxRate products', async () => {
    await expect(
      calculateTaxExcludedPrice(
        TaxationCategory.ReducedTaxRate,
        1000,
        null,
        null,
        'TestScreen'
      )
    ).rejects.toThrow('軽減された税率が取得できません')
  })

  test('should return 0 for TransitionalMeasures products', async () => {
    const result = await calculateTaxExcludedPrice(
      TaxationCategory.TransitionalMeasures,
      1000,
      8,
      5,
      'TestScreen'
    )
    expect(result).toEqual(0)
  })

  test('should throw an error if taxation category is unknown', async () => {
    await expect(
      calculateTaxExcludedPrice('Unknown', 1000, 8, 5, 'TestScreen')
    ).rejects.toThrow('課税区分が不明です')
  })
})

```
