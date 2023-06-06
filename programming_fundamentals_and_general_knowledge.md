# 一般知識

## /helper フォルダ

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

## /common フォルダ

- プロジェクト全体で共有されるコンポーネントや定数、型定義などが含まれることが多い

```js:例
- Button.vue: プロジェクト全体で使用されるボタンコンポーネント。
- colors.ts: プロジェクト全体で使用される色の定数。
- types.d.ts: プロジェクト全体で使用される型定義。
```
