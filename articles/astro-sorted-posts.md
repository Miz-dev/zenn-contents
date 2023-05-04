---
title: "Astroで最新記事から日付順に記事一覧を表示する方法"
emoji: "🚀"
type: "tech"
topics: ["Astro", "JavaScript", "ChatGPT", "初心者"]
published: false
---

# はじめに

どうも Miz です。  
Astro ブログを作成したので記事を投稿していると、記事一覧で最新記事が下に追加されていました。  
最新記事から日付順に記事一覧を表示するのどうやるんだっけ？と思い、せっかくなので ChatGPT に訊きながら実装してみました。

:::message
ここはもっとこう実装した方が良いよ！ってのがありましたらコメントで教えて下さい！
:::

## 余談

ChatGPT で解決方法を訊きながら実装していって、最後に  
「ここまでの内容をブログ記事にまとめたいです。その記事を作成してください。」  
といって初めて記事作成してみましたが、この方法良さそう...

# 最新記事から日付順に記事一覧を表示

Astro では、Markdown ファイルの Frontmatter から日付情報を取得して記事一覧を生成することができます。この記事では、最新記事から日付順に記事一覧を表示する方法を説明します。

## 1. 記事一覧を取得する

まず、以下のように Astro.glob()を使用して、記事一覧を取得します。

```jsx
const allPosts = await Astro.glob("../pages/posts/_.md");
const nonDraftPosts = allPosts.filter((post) => !post.frontmatter.draft);
```

このコードでは、Astro.glob()を使用して、../pages/posts/\_.md にマッチする Markdown ファイルのリストを取得しています。filter()を使用して、draft が true でない記事のみを取得しています。

## 2. 日付順にソートする

次に、記事を日付順にソートします。以下のように、sort()メソッドを使用して、pubDate の値に基づいて記事を並び替えます。

```jsx
const sortedPosts = nonDraftPosts.sort((a, b) => {
  const aDate = new Date(a.frontmatter.pubDate);
  const bDate = new Date(b.frontmatter.pubDate);
  return bDate.getTime() - aDate.getTime();
});
```

このコードでは、各記事の pubDate を new Date()で日付オブジェクトに変換し、getTime()を使用して、各日付オブジェクトのミリ秒数を取得しています。そして、それらの差を計算して、結果を返しています。

## 3. 記事一覧を表示する

最後に、以下のように、map()を使用して、記事一覧を表示します。

```jsx
<ul>
  {sortedPosts.map((post) => (
    <Post
      url={post.url}
      title={post.frontmatter.title}
      pubDate={post.frontmatter.pubDate}
      tags={post.frontmatter.tags}
    />
  ))}
</ul>
```

このコードでは、sortedPosts の各記事に対して、<Post>コンポーネントを使用して、リストアイテムを生成します。

# 最後に

以上で、Astro で最新記事から日付順に記事一覧を表示する方法を説明しました。  
これを使って、より使いやすく魅力的なブログを作成してください。

## Astro で作成したブログ

MizDev
https://mizdev.vercel.app/
