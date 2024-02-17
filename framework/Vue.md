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

<br />
<br />

## **<font color="#00ff00">computed()</font>**

[公式 docs](https://ja.vuejs.org/api/reactivity-core.html#computed)

レビューでいただいたコメント<br>
src/components/group/group-activities.vue

```ts
const selectedIsShowCommentFilterItem = ref(
  localStorage.getItem("selectedIsShowCommentFilterItem") ?? "showComment"
);
const isShowArticleCardComment = ref(
  selectedIsShowCommentFilterItem.value === "showComment"
);
```

(nits) handleArticleCardCommentOpen() 内で更新するのを、selectedIsShowCommentFilterItem の方にして、isShowArticleCardComment は computed にすると意味合いが分かりやすくなりそうです。

const isShowArticleCardComment = computed(() =>
selectedIsShowCommentFilterItem.value === 'showComment'
);
情報ソースとなる値は ref にして、それに付随して変更される値は computed なるように変数を整理できると、うまいこと状態を管理できると思いますー。

<br><br>

ゲッター関数を受け取り、ゲッターからの戻り値に対して読み取り専用のリアクティブな ref オブジェクトを返す

```ts
// 読み取り専用
function computed<T>(
  getter: () => T,
  // 下記の "Computed Debugging" リンクをご参照ください
  debuggerOptions?: DebuggerOptions
): Readonly<Ref<Readonly<T>>>;

--------------------------------------

// 例) 読み取り専用の算出 ref の作成:
const count = ref(1);
const plusOne = computed(() => count.value + 1);

console.log(plusOne.value); // 2

plusOne.value++; // エラー
```

<br>

また、get 関数と set 関数を持つオブジェクトを受け取り、書き込み可な ref オブジェクトを作成することができる

```ts
// 書き込み可
function computed<T>(
  options: {
    get: () => T;
    set: (value: T) => void;
  },
  debuggerOptions?: DebuggerOptions
): Ref<T>;

--------------------------------------

// 例) 書き込み可の算出 ref の作成:
const count = ref(1);
const plusOne = computed({
  get: () => count.value + 1,
  set: (val) => {
    count.value = val - 1;
  },
});

plusOne.value = 1;
console.log(count.value); // 0
```

<br>

デバッグ

```ts
const plusOne = computed(() => count.value + 1, {
  onTrack(e) {
    debugger;
  },
  onTrigger(e) {
    debugger;
  },
});
```

<br><br>

## **<font color="#00ff00">vee-validate と yup を用いたバリデーション実装</font>**

[vee-validate 公式](https://vee-validate.logaretm.com/v4/api/use-form/)

[vee-validate GitHub リポジトリ](https://github.com/logaretm/vee-validate)

[yup GitHub リポジトリ](https://github.com/jquense/yup)

【詰まったポイント】

useForm と useField で script タグ内でどちらも定義する場合、useField を useForm の後に記載しないと正しくバリデーションが動かなかった。
下記の例では必須項目のエラーが常に出続けていた。

- おかしな挙動をしてた例

```TypeScript
/* 入力値 */
const { value: email } = useField<string>('email')
const { value: emailConfirmation } = useField<string>('emailConfirmation')
const { value: password } = useField<string>('password')
const { value: passwordConfirmation } = useField<string>('passwordConfirmation')

/* 入力値スキーマ */
const schema = object({
  email: string()
    .email(emailMessage)
    .matches(REGEXP.EMAIL, emailMessage)
    .max(80, maxLengthMessage(80))
    .required(requiredMessage),
  emailConfirmation: string()
    .email(emailMessage)
    .matches(REGEXP.EMAIL, emailMessage)
    .max(80, maxLengthMessage(80))
    .oneOf([yupRef('email'), null, undefined, ''], emailNotMatchMessage)
    .required(requiredMessage),
  password: string()
    .min(8, minLengthMessage(8))
    .max(12, maxLengthMessage(12))
    .matches(REGEXP.PASSWORD, passwordMessage)
    .required(requiredMessage),
  passwordConfirmation: string()
    .min(8, minLengthMessage(8))
    .max(12, maxLengthMessage(12))
    .matches(REGEXP.PASSWORD, passwordMessage)
    .oneOf([yupRef('password'), null, undefined, ''], passwordNotMatchMessage)
    .required(requiredMessage),
})

/* Fieldで作成されたフィールドをグループ化 */
const { handleSubmit, validate, values, errors } = useForm({
  validationSchema: schema,
})
```

- 正しく動いた例

```TypeScript
/* 入力値スキーマ */
const schema = object({
  email: string()
    .email(emailMessage)
    .matches(REGEXP.EMAIL, emailMessage)
    .max(80, maxLengthMessage(80))
    .required(requiredMessage),
  emailConfirmation: string()
    .email(emailMessage)
    .matches(REGEXP.EMAIL, emailMessage)
    .max(80, maxLengthMessage(80))
    .oneOf([yupRef('email'), null, undefined, ''], emailNotMatchMessage)
    .required(requiredMessage),
  password: string()
    .min(8, minLengthMessage(8))
    .max(12, maxLengthMessage(12))
    .matches(REGEXP.PASSWORD, passwordMessage)
    .required(requiredMessage),
  passwordConfirmation: string()
    .min(8, minLengthMessage(8))
    .max(12, maxLengthMessage(12))
    .matches(REGEXP.PASSWORD, passwordMessage)
    .oneOf([yupRef('password'), null, undefined, ''], passwordNotMatchMessage)
    .required(requiredMessage),
})

/* Fieldで作成されたフィールドをグループ化 */
const { handleSubmit, validate, values, errors } = useForm({
  validationSchema: schema,
})

/* 入力値 */
const { value: email } = useField<string>('email')
const { value: emailConfirmation } = useField<string>('emailConfirmation')
const { value: password } = useField<string>('password')
const { value: passwordConfirmation } = useField<string>('passwordConfirmation')
```

ref の値のように script タグの上の方で定義したかったから、
useForm より先に useField を書いてたけど正しく動作しなかった。
時間ある時になんでそんな挙動したか vee-validate の処理の流れを追ってみよう

<br><br>

## **<font color="#00ff00">Deep Selectors</font>**

親コンポーネントから子コンポーネントのスタイルを上書くようなことができる

```js
<style scoped>.a :deep(.b) {/* ... */}</style>
```

https://vuejs.org/api/sfc-css-features.html#deep-selectors
https://zenn.dev/kibe/articles/afd8162d5f6fe9

<br><br>

## **<font color="#00ff00">Setup Context</font>**

【公式記事】

https://ja.vuejs.org/api/composition-api-setup.html#setup-context

<br>

【GitHub Copilot への質問】

```
この親コンポーネントに渡すイベント発火の書き方で setup(props, context)のcontextはなんですか？vueでいうところのどういった機能でどう言う書き方をしているか具体的かつ詳細に説明してください

------------------------------------------------------------------------
【GitHub Copilot】
Vue 3のComposition APIのsetup関数では、第二引数としてcontextオブジェクトが提供されます。このcontextオブジェクトは、Vue 2のthisコンテキストに相当するもので、以下の3つのプロパティを持っています。

attrs: 親から渡された属性を含むリアクティブなオブジェクトです。これは、親コンポーネントが子コンポーネントに渡す属性（propsで定義されていないもの）を参照するために使用します。

slots: 親から渡されたスロットを含むオブジェクトです。これは、親コンポーネントが子コンポーネントに渡すスロットを参照するために使用します。

emit: イベントを発火するための関数です。これは、子コンポーネントが親コンポーネントにイベントを通知するために使用します。

このcontext.emit関数を使用すると、子コンポーネントは親コンポーネントに対してカスタムイベントを発火することができます。この関数は2つの引数を取ります。第一引数はイベント名（文字列）、第二引数はイベントのペイロード（任意のデータ）です。

例えば、以下のように使用することができます。

このコードは、my-eventという名前のイベントを発火し、そのペイロードとしてオブジェクトを送信します。親コンポーネントはこのイベントをリッスンし、ペイロードを受け取ることができます。
```

<br><br>

## **<font color="#00ff00">Emit の型付け</font>**

- オプションベースの宣言
- 型ベースの宣言
- Vue3.3 以降からの簡潔な構文

などいくつかの記法がある（Vue が 3.3 以降のバージョンなら最新の書き方で良さそう）

```vue
<script setup lang="ts">
// 実行時の宣言
const emit = defineEmits(["change", "update"]);

// オプションベースの宣言
const emit = defineEmits({
  change: (id: number) => {
    // バリデーションの合格/不合格を示すための
    // `true` または `false` を返す
  },
  update: (value: string) => {
    // バリデーションの合格/不合格を示すための
    // `true` または `false` を返す
  },
});

// 型ベースの宣言
const emit = defineEmits<{
  (e: "change", id: number): void;
  (e: "update", value: string): void;
}>();

// 3.3+: より簡潔な代替の構文
const emit = defineEmits<{
  change: [id: number];
  update: [value: string];
}>();
</script>
```

以下記事から抜粋<br>
https://ja.vuejs.org/guide/typescript/composition-api#typing-component-emits

<br><br>

## **<font color="#00ff00">provide / inject</font>**

階層を跨いで値の受け渡しができる機能。

ここでは、provide と inject を使用して、3 階層上の親コンポーネントで定義された状態を孫コンポーネントで更新する具体的な例を示します。この例では、ポップアップの表示状態を親コンポーネントで管理し、孫コンポーネントからその状態を変更します。

親コンポーネント (ParentComponent.vue)

```
vue
Copy code
<template>   <div>     <ChildComponent />   </div> </template> <script setup> import { ref } from 'vue'; import ChildComponent from './ChildComponent.vue'; const isOpen = ref(false); // ポップアップの表示状態と制御メソッドを提供 provide('popupState', isOpen); provide('togglePopup', () => isOpen.value = !isOpen.value); </script>
```

子コンポーネント (ChildComponent.vue)

```
vue
Copy code
<template>   <GrandchildComponent /> </template> <script setup> import GrandchildComponent from './GrandchildComponent.vue'; </script>
```

孫コンポーネント (GrandchildComponent.vue)

```
vue
Copy code
<template>   <div>     <button @click="togglePopup">Toggle Popup</button>     <div v-if="isOpen">This is a popup!</div>   </div> </template> <script setup> import { inject } from 'vue'; // 親コンポーネントから提供された状態とメソッドを注入 const isOpen = inject('popupState'); const togglePopup = inject('togglePopup'); </script>
```

この例では、ParentComponent.vue が isOpen というリアクティブなデータと togglePopup という関数を提供しています。GrandchildComponent.vue はこれらを inject して使用し、ボタンクリックでポップアップの表示状態を切り替えます。ChildComponent.vue は中間のコンポーネントで、特に provide や inject に関与していませんが、親から孫へのデータの流れを示すために存在します。

この構成により、孫コンポーネントからポップアップの表示状態を制御でき、その状態の変更が親コンポーネントに伝播します。provide と inject を使用することで、親子間、またはより深い階層間でのデータの受け渡しが可能になり、Vue.js アプリケーションの状態管理が柔軟になります。

<br><br>

## **<font color="#00ff00">props の定義方法とデフォルト値</font>**

```ts
<script setup lang="ts">
import { ref, watch } from 'vue';
import { defineEmits, defineProps } from 'vue';

interface Props {
  placeholder?: string;
  disableSearchIcon?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: '検索…',
  disableSearchIcon: true
});
```

<br><br>

## **<font color="#00ff00">Vue Test Unit</font>**

### shallowMount と mount の違い

- どちらもマウントを行うための関数
- 主な違いは子コンポーネントの扱い方

### 【mount】

テスト対象のコンポーネントとそのすべての子コンポーネントをフルレンダリングする。<br>
つまり、子コンポーネントもその内部のすべての要素とともにレンダリングされる

### 【shallowmount】

一方、shallowMount は、テスト対象のコンポーネントのみをフルレンダリングし、子コンポーネントはスタブ（ダミーの置き換え）としてレンダリングする。
これにより、テスト対象のコンポーネントのみに焦点を当て、その外部依存関係を排除することができる。

したがって、shallowMount は、テスト対象のコンポーネントが他のコンポーネントとどのように相互作用するかではなく、その内部のロジックをテストするのに適しています。
