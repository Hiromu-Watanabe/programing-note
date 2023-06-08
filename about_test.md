# テストについて

<br />
<br />

# **<font color="#00ff00">Vitest</font>**

<img src="./images/Vitest_image.png" width="100%" alt="Vue.jsのイメージ">

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

```ts: vitestテストコードの例
import { mount } from '@vue/test-utils'
import MenusCarousel from '@/components/carousel/MenusCarousel.vue'
import { assert, test } from 'vitest'

// Note: No describe method in Vitest
test('MenusCarousel.vue', async () => {
  const wrapper = mount(MenusCarousel)

  // Get the exposed function.
  const { toYen } = wrapper.vm

  // Test the toYen function.
  assert.strictEqual(toYen(1234567890), '1,234,567,890')
})
```
