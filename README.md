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
