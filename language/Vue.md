# Vue.js

<img src="../images/Vue_image.png" width="100%" alt="Vue.jsのイメージ">

現在時点（2023/6/7）のプロジェクトでは vue3 の CompositionAPI を使用しているため、
vue3 の CompositionAPI に沿った内容について書いていく

[公式サイト](https://vuejs.org/)

<br />
<br />

---

## **<font color="#00ff00">v-slot</font>**

Vue.js で[スロット](https://developer.mozilla.org/ja/docs/Web/HTML/Element/slot)を利用するためのディレクティブ

Vue.js 2.6.0 以上からスロットの新しい構文として導入されました。

**例）**

【親コンポーネント】

```vue:こんな使い方もある
<WithSoldout
  v-if="reserve"
  :callenderId="reserve?.callender ?? ''"
  :productId="items.productId"
>
  <template v-slot="soldoutProps">
    {{
      soldoutProps.isLoading
        ? '-'
        : soldoutProps.isSoldout
        ? '売り切れ'
        : `¥${switchTax(items)}`
    }}
  </template>
</WithSoldout>
```

【子コンポーネント】

```vue:こんな使い方もある
<template>
  <slot
    v-bind:isSoldout="results.isSoldout"
    v-bind:isLoading="results.isLoading"
  ></slot>
</template>

<script setup lang="ts">
import { onMounted, reactive } from 'vue'

const props = defineProps<{
  callenderId: string
  productId: string
}>()

const results = reactive<{ isSoldout: boolean; isLoading: boolean }>({
  isLoading: false,
  isSoldout: false,
})

onMounted(async () => {
  try {
    results.isLoading = true
    const is = await isSoldout(props.callenderId, props.productId)
    results.isSoldout = is == null ? false : is
  } catch (e) {
    if (isGraphQLError(e)) error(wrapGraphQLError(e))
    if (e instanceof Error) error(e.message)
    throw CommonError
  } finally {
    results.isLoading = false
  }
})
</script>
```

**【上記コード解説】**

ここでの v-slot="soldoutProps"は、WithSoldout コンポーネントが提供するスロットプロップ（スロットの中で利用可能なデータ）を soldoutProps という名前で受け取っています。スロットプロップを用いることで、親コンポーネントから子コンポーネントへデータを渡すことができます。

この例では、WithSoldout コンポーネントから isLoading と isSoldout というプロパティを含むオブジェクト（スロットプロップ）を受け取っています。これらのプロパティを利用して、ロード中は'-'を表示し、売り切れていれば'売り切れ'を表示し、それ以外の場合はアイテムの価格を表示しています。

スロットとスロットプロップを使用することで、コンポーネント間でのデータの受け渡しとコードの再利用性を高めることができます。

<br />
<br />

---

## **<font color="#00ff00">watchEffect()</font>**

Vue3 の Composition API には、watchEffect という機能があります。watchEffect は、リアクティブな状態の変更を監視し、その変更に応じて副作用を実行することができます。[^1]

[^1]: https://qiita.com/doz13189/items/d09cfc6e1ff38621c2cc

例えば、次のようなコードがあります。

```vue:watchEffectの例
<script setup lang="ts">
import { ref, watchEffect } from 'vue';

const count = ref(0);
let sum = 0;

watchEffect(() => {
  sum = count.value + 1
})
</script>
```

このコードでは、count というリアクティブな変数の値が変更される度にそれを検知して、watchEffect 関数が実行される。
そのため count の値が変更されるたびに、sum の値も更新されます。

このように、watchEffect はリアクティブな状態の変更を監視し、その変更に応じて副作用を実行することができる機能です。

<br />
<br />

computed でも同じようなことが出来るが、computed では非同期関数を使用できない。
リアクティブに変更を検知して値を更新する処理に非同期関数が含まれる場合は watchEffect と ref を併用することで実現できる。

```vue:watchEffectとrefの組み合わせ
<script setup lang="ts">
const reservedMenus = ref<OrderModel[]>([]) // 非同期処理の結果を格納するためのリアクティブな変数
/* 注文済商品 */
watchEffect(async () => {
  const orders = (reserve.value?.orders?.items ?? [])
    .filter(() => /* フィルターやソート */)

  const asyncOrders: OrderModel[] = await Promise.all(
    orders.map(async (menu) => ({
      ...menu,
      product: { ...menu.product, price: await getPrice(menu.product) },
    }))
  )

  reservedMenus.value = asyncOrders // 非同期処理の結果をリアクティブな変数に格納
})
</script>
```

<br />
<br />

---

## **<font color="#00ff00">v-on</font>**

`v-on` ディレクティブを使用することで、 DOM イベントの購読やイベント発火時にいくつかの JavaScript を実行します。これは通常 @ に省略することができます。

使い方は `v-on:click="handler"`、あるいは省略して `@click="handler"` として使用します。

ハンドラーの値は以下のいずれかを指定します:

1. 【インラインハンドラー】

   イベント発火時に実行されるインライン JavaScript 式 (これはネイディブの `onclick` 属性に似たものです)

   ```vue:インラインハンドラー例
   <template>
     <button @click="count++">Add 1</button>
     <p>Count is: {{ count }}</p>
   </template>

   <script setup lang="ts">
     import { ref } from 'vue'

     const count = ref(0)
   </script>
   ```

2. 【メソッドハンドラー】

   コンポーネント上で定義されたメソッドを示すプロパティ名またはパス

   ```vue:インラインハンドラー例
   <template>
    <!-- `greet` はscriptタグ内で定義したメソッド名です。 -->
    <button @click="greet">Greet</button>
   </template>

   <script setup lang="ts">
    import { ref } from 'vue'

    const name = ref('Vue.js')

    function greet(event) {
      alert(`Hello ${name.value}!`)
      // `event` はネイディブ DOM イベントです。
      if (event) {
        alert(event.target.tagName)
      }
    }
   </script>
   ```
