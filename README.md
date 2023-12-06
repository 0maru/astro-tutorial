# Astro Tutorial

## 1. ディレクトリを作成する

```:shell
mkdir astro-tutorial
cd astro-tutorial
```

## 2. プロジェクトを作成する

```:npm
npm create astro@latest .
```

```:shell
How would you like to start your new project?
>> Empty

Install dependencies?
>> Yes

Do you plan to write TypeScript?
>> yes

How strict should TypeScript be?
>> Strict

Initialize a new git repository?
>> No
```

## 3.　Astro を起動する

```:npm
npm run dev
```

## 4. エディタでプロジェクトを開く

VS Code とWebStorm はプラグインを追加することでハイライトなどがサポートされる

## 5. 適当に文字を書き換える

pages/index.astro を開く

```:html
<html lang="en">
 <head>
  <meta charset="utf-8" />
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <meta name="viewport" content="width=device-width" />
  <meta name="generator" content={Astro.generator} />
  <title>Astro</title>
 </head>
 <body>
  <h1>ブログ</h1>
 </body>
</html>
```

## 6. コンポーネントを作成する

1. components ディレクトリを作成
2. Button.astro を作成

```html
---

---
<button>
    送信
</button>
```

3. index.astro で作成したボタンを読み込む

```:html
---
import Button from "../components/Button.astro";
---

<html lang="en">
 <head>
  <meta charset="utf-8" />
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <meta name="viewport" content="width=device-width" />
  <meta name="generator" content={Astro.generator} />
  <title>Astro</title>
 </head>
 <body>
  <h1>ブログ</h1>
  <Button>
 </body>
</html>

```

4. JavaScriptを実行できるようにする

```:html
---

---
<button submit-button>
    送信
</button>

<script>
    const button = document.querySelector('[submit-button]');
    button.addEventListener('click', () => {
        alert('送信しました');
    });
</script>
```

## 7. JSXを使う

「6. コンポーネントを作成する」で作成したボタンが props を受け取れるようにする
「6. コンポーネントを作成する」で作ったボタンだと同じdata属性のボタンが複数個生成されてどのボタンを押したか分からなくなるので、
別の方法でクリックされたボタンを特定できるようにする。

WebComponents をカスタム要素を使用することで、`document.querySelector()`だと`document`のすべてのDOMが検索対象だが、
`this.querySelector()` で個別にコンポーネントを操作することができる。

```:html
---
interface ButtonProps {
    index: number;
    label: string;
}

const { index, label } = Astro.props;
---
<!-- data属性にprops の値を渡すことでscript 内で使用できるようになる -->
<submit-button data-label={label}>
    <button>
        {label}
    </button>
</submit-button>

<script>
    class SubmitButton extends HTMLElement {
        constructor() {
            super();

            const button = this.querySelector('button');
            // this.dataset で data属性の値を取得できる
            const label = this.dataset.label
            button.addEventListener('click', () => {
                alert(`「${label}」ボタンがクリックされました`);
            });
        }
    }
    // submit-button タグに SubmitButton クラスを紐付ける
    customElements.define('submit-button', SubmitButton);
</script>
```

pages/index.astro にJSXでボタンを複数個表示できるようにする

```:html
---
import Button from "../components/Button.astro";

const buttonLabels: string[] = ["ボタン1", "ボタン2", "ボタン3"];
---

<html lang="en">
 <head>
  <meta charset="utf-8" />
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <meta name="viewport" content="width=device-width" />
  <meta name="generator" content={Astro.generator} />
  <title>Astro</title>
 </head>
 <body>
  <h1>ブログ</h1>
  {buttonLabels.map((label, idx) => <Button index={idx} label={label} />)}
 </body>
</html>
```
