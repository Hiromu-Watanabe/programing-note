# Next.js

<br />

## **<font color="#00ff00">Next.js のディレクトリ構成</font>**

[参考記事 1](https://zenn.dev/necscat/articles/d5d9b7a3f859d7)
[参考記事 2](https://note.com/ryoppei/n/n2e3e7a66e758)

<br />

## **<font color="#00ff00">Next.js v13 で Chakra UI の Link, Button を使う Tips</font>**

[こちらの記事を拝借](https://chaika.hatenablog.com/entry/2023/03/26/083000)

### 環境

- Next.js `13.4.19`
  - React `18.2.0`
- @chakra-ui/react `2.8.0`
- TypeScript `5.2.2`

<br>

### 結論: Next.js v13 で Chakra UI を使う場合 Link は `@chakra-ui/next-js` を使う、リンクボタンは `as={NextLink}` にする

<br>

### Link: `@chakra-ui/next-js`を使う

cf. [Getting Started with Next.js - Chakra UI](https://chakra-ui.com/getting-started/nextjs-guide#chakra-ui-with-nextjs-link)

```shell
$ nom i @chakra-ui/next-js
```

```typescript
- import NextLink from 'next/link';
- import { Link } from '@chakra-ui/react';
+ import { Link } from '@chakra-ui/next-js'

export default Page(): JSX.Element {
  return (
-   <NextLink href='/page' passHref>
-     <Link>Link Text</Link>
-   </NextLink>
+   <Link href="/page">Link Text</Link>
  );
}
```

### Button: `as={NextLink}`を使う

`passHref` を使うのではなく Chakra UI の as に `NextLink` を指定すれば NextLink な ボタンになる

```typescript
import NextLink from 'next/link';
import { Button } from '@chakra-ui/react';

export default Page(): JSX.Element {
  return (
-   <NextLink href='/page' passHref>
-     <Button as="a">Link Text</Button>
-   </NextLink>
+   <Button as={NextLink}>Link Text</Button>
  );
}
```
