# Vue.js

<img src="../images/Vue_image.png" width="100%" alt="Vue.jsのイメージ">

現在時点（2023/6/7）のプロジェクトでは vue3 の CompositionAPI を使用しているため、
vue3 の CompositionAPI に沿った内容について書いていく

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

<br />
<br />

---
