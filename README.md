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
>> No

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

## 8. ページを追加する

src/pages にファイルを追加することでページを使いできる

`.astro`, `.md`, `.mdx`, `.html` の拡張子に対応している

pages/ の下に`about.astro` を作成する

```:html
---

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
  <h1>私について</h1>
 </body>
</html>
```

<http://localhost:4321/about> にアクセスするとページが作成されていることが確認できる

## 9. ブログ用のディレクトリを作成する

`src/pages` の下に `blogs` ディレクトリを作成してください。

## 10. ブログ記事用のファイルを作成する

`src/page/blogs` に `first.md` ファイルを作成してください。
マークダウンで書いたファイルがHTMLに変換されて表示されます。

```:markdown
---
title: 初めてのブログ
---

# 初めてのブログ

この記事はこのブログの初めての記事です。

## リスト

- リストも書けます
    - 深いリストも書けます
        - 更に深いリストも書けます
- リストも書けます？
- リストも書けます！

## テーブル

| ある   | なし     |
|------|--------|
| クラス  | ホームルーム |
| 心臓   | 肝臓     |
| 猿    | 狼      |
| かっこう | ほととぎす  |
| こま   | まり     |

## 引用

> 引用はこんな感じで表示されます
```

<http://localhost:4321/blogs/first> にアクセスする

## 11. レイアウトを作成する

`src` の下に `layouts` ディレクトリを作成する。
import して使用するのでディレクトリ名に制限はない。

作成した`layouts` ディレクトリの下に `Blog.astro` を作成する。

```:html
---
interface Props {
  frontmatter: {
    title: string;
    description: string;
    createdAt: string;
  };
}

const { frontmatter } = Astro.props;
---
<!doctype html>
<html lang="ja">
  <head>
    <BaseHead title={title} description={description} />
  </head>
  <body>
    <main>
      <slot />
    </main>
  </body>
</html>
```

## 12. トップページにブログ記事一覧を表示する

```:html
---
const allBlogs = await Astro.glob("../pages/blogs/*.md");
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
  {
   allBlogs.map((blog, idx) => {
    return <p>{blog.frontmatter.title}</p>;
   })
  }
 </body>
</html>
```

2つ目のブログ記事を追加する

```:markdown
---
title: 2つ目の記事
layout: ../../layouts/Blog.astro
---

# 2つ目の記事

2つ目の記事です。

```

トップページに2つめの記事が表示される

## 13. 記事のページに遷移できるようにする

pタグをaタグに変更して遷移できるようにする

```:html
---
const allBlogs = await Astro.glob("../pages/blogs/*.md");
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
  {
   allBlogs.map((blog) => {
    return <a href={blog.url} style="display: block">{blog.frontmatter.title}</a>;
   })
  }
 </body>
</html>
```

## 14. ブログ記事にタグを追加してタグ一覧ページを作成する

`fisrt.md` にタグを追加する

```:markdown
---
title: 初めてのブログ
layout: ../../layouts/Blog.astro
tags: ['Tech', 'Astro']
---
```

`second.md` にタグを追加する

```:markdown
---
title: 2つ目の記事
layout: ../../layouts/Blog.astro
tags: ['Tech', 'TypeScript', 'Blog']
---
```

`pages` の下に `tags` ディレクトリを作成する。

`tags`　の下に `index.astro` を作成する。

```:html
---
const allBlogs = await Astro.glob("../blogs/*.md");
const tags = [...new Set(allBlogs.map((post) => post.frontmatter.tags).flat())];
---

<!doctype html>
<html lang="ja">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width,initial-scale=1" />
    </head>
    <body>
        <main>
            <h1>タグ一覧</h1>
            <ul>
                {
                    tags.map((tag) => {
                        return <li>{tag}</li>;
                    })
                }
            </ul>
        </main>
    </body>
</html>
```

# 15. タグで記事を絞り込んだページを作成する

動的なパスは `[変数名].astro`  のファイル名でファイルを作るとできる。

```:html
---
export async function getStaticPaths() {
    const allBlogs = await Astro.glob("../blogs/*.md");
    const tags = [
        ...new Set(allBlogs.map((blog) => blog.frontmatter.tags).flat()),
    ];
    return tags.map((tag) => ({ params: { tag }, props: { blogs: allBlogs } }));
}

const { tag } = Astro.params;
const { blogs } = Astro.props;
const filterdBlogs = blogs.filter((blog) =>
    blog.frontmatter.tags.includes(tag),
);
---

<!doctype html>
<html lang="ja">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width,initial-scale=1" />
    </head>
    <body>
        <main>
            <h1>タグ一覧</h1>
            <ul>
                {
                    filterdBlogs.map((blog) => {
                        return (
                            <li>
                                <a href={blog.url}>{blog.frontmatter.title}</a>
                            </li>
                        );
                    })
                }
            </ul>
        </main>
    </body>
</html>
```

`pages/tags/index.astro` を書き換えてタグで絞り込んだページに遷移できるようにする

```:html
---
const allBlogs = await Astro.glob("../blogs/*.md");
const tags = [...new Set(allBlogs.map((post) => post.frontmatter.tags).flat())];
---

<!doctype html>
<html lang="ja">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width,initial-scale=1" />
    </head>
    <body>
        <main>
            <h1>タグ一覧</h1>
            <ul>
                {
                    tags.map((tag) => {
                        return (
                            <li>
                                <a href={`/tags/${tag}`}>{tag}</a>
                            </li>
                        );
                    })
                }
            </ul>
        </main>
    </body>
</html>
```

## 16. CSS フレームワークを追加してレイアウトを整える

```:shell
npx astro add tailwind
```
