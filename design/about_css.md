# CSS

<img src="../images/CSS_image.jpg" width="100%" alt="Gitのイメージ">

## **<font color="#00ff00">drop-shadow()</font>**

入力画像にドロップシャドウ効果を適用する

```css
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
```

<br>

## **<font color="#00ff00">text-overflow: ellipsis</font>**

はみ出たテキストの表示方法を指定するプロパティ

参考記事 : https://www.osaka-kyoiku.ac.jp/~joho/html5_ref/css/text-overflow_css.php?

```html
<html>
  <div class="sample tovf1">
    ああああああああああああああああああああああああああ
  </div>
  <div class="sample tovf2">
    ああああああああああああああああああああああああああ
  </div>
  <div class="sample tovf3">
    ああああああああああああああああああああああああああ
  </div>
  <div class="sample tovf4">
    ああああああああああああああああああああああああああ
  </div>
</html>

<style>
  div.sample {
    width: 300px;
    overflow: hidden;
    white-space: nowrap;
    padding: 5px;
    margin: 10px;
    border: 2px solid #666;
  }
  .tovf1 {
    text-overflow: clip;
  }
  .tovf2 {
    text-overflow: ellipsis;
  }
  .tovf3 {
    text-overflow: ".";
  }
  .tovf4 {
    text-overflow: "";
  }
</style>
```
