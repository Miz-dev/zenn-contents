---
title: "Tailwind CSS V4まとめ！"
emoji: "📚"
type: "tech"
topics: [tailwindcss, css, react, javascript, nextjs]
published: true
---

# はじめに

Tailwind CSS v4.0は、パフォーマンスと柔軟性のために最適化されたフレームワークの新バージョンで、設定とカスタマイズのエクスペリエンスが一新された。  

## TL;DR
■主な変更点
- パフォーマンスの大幅な向上
- コンテンツの自動検出
- CSSファースト設定
- arbitrary valuesが簡素化された

JavaScriptではなくCSSでプロジェクトを設定するようになったのが、一番大きい。（これは革命）  
あとは個人的にはarbitrary valuesが簡素化されたのは書きやすくはなるけど、秩序としては崩れやすいのでこの変更点はあまり好きではない...😢

## 新しい高性能エンジン
Tailwind CSS v4.0は、Rust製の新しいビルドエンジンOxideを導入され、フレームワークをゼロから書き直し、パフォーマンスを大幅に向上させた。

主な改善点は以下の通り。
- フルビルド: 3.5倍以上高速化
- インクリメンタルビルド: 8倍以上高速化
- 新しいCSSを含まないインクリメンタルビルド: 100倍以上高速化（マイクロ秒単位で完了）

特に、既存のクラスを再利用するだけのインクリメンタルビルドは、100倍以上高速化され、マイクロ秒単位で完了するようになった。  
プロジェクトの規模が大きくなり、既存のクラスを再利用するケースが増えるほど、この高速化の効果は大きくなる。

## モダンウェブのためのデザイン
Tailwind CSS v4.0は、最新のWeb技術を活用して進化した。  

主な改善点は以下の通り。
- カスケードレイヤー: スタイルルールの相互作用をより細かく制御できるようになった。
- 登録済みカスタムプロパティ: グラデーションのアニメーションなどを実現し、大規模ページのパフォーマンスを大幅に向上。
- color-mix(): CSS変数やcurrentColorを含むあらゆるカラー値の不透明度を調整できるようになった。
- 論理プロパティ: RTLサポートを簡素化し、生成されるCSSのサイズを削減。

これらの機能により、Tailwindの内部構造も簡素化され、バグ発生の可能性を減らし、フレームワークのメンテナンスが容易になった。

## インストールプロセスが大幅に簡素化
Tailwind CSS v4.0では、インストールプロセスが大幅に簡素化された。  

主な改善点は以下の通り。
- 手順の削減: 必要な手順が減り、ボイラープレートも削除された。
- CSSは1行だけ: `@tailwind`ディレクティブは不要になり、`@import "tailwindcss"`を追加するだけで使用できるようになった。
- 設定不要: テンプレートファイルへのパスなどを設定する必要が無くなった。
- 外部プラグイン不要: `@import`ルールはデフォルトでバンドルされ、ベンダープレフィックスや構文変換にはLightning CSSが使用されるようになった。

これらの改善により、Tailwindはこれまで以上に軽量になり、プロジェクトの初期設定が簡単になった。

## ファーストパーティ製Viteプラグイン
Viteユーザー向けに、PostCSSの代わりに`@tailwindcss/vite`を使用してTailwindを統合できるようになった。
```ts
// vite.config.ts
import { defineConfig } from "vite";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [
    tailwindcss(),
  ],
});
```
Tailwind CSS v4.0はPostCSSプラグインとしても非常に高速だが、Viteプラグインを使用するとさらにパフォーマンスが向上する。

## コンテンツの自動検出
Tailwind CSS v4.0では、コンテンツの自動検出機能が追加された。
以前は`content`配列に自分でファイルパスを指定する必要があったが、v4.0ではヒューリスティックを使用して自動的に検出されるようになった。

主な改善点は以下の通り。
- `.gitignore`ファイルに記載されているファイルは自動的に無視される。

```.ignore
/node_modules
/coverage
/.next/
/build
```

- 画像、動画、.zipファイルなどのバイナリファイルも自動的に無視される。
- `@source`ディレクティブを使用して、デフォルトで除外されるソースを明示的に追加できる。

```css
@import "tailwindcss";
@source "../node_modules/@my-company/ui-lib";
```
`@source`ディレクティブでもヒューリスティックが使用されるため、バイナリファイルなどを明示的に指定する必要が無くなった。

## CSSファイルのインライン化が組み込みでサポート
Tailwind CSS v4.0では、`@import`によるCSSファイルのインライン化が組み込みでサポートされるようになった。
以前は`postcss-import`のようなプラグインが必要だったが、v4.0ではTailwind CSS自体が`@import`を処理できるようになった。

```js
// postcss.config.js
export default {
  plugins: [
    // "postcss-import", // これが不要に
    "@tailwindcss/postcss",
  ],
};
```

Tailwind CSSのエンジンと緊密に統合されているため、`@import`の処理速度も向上している。

## CSSファーストの構成
Tailwind CSS v4.0では、JavaScriptではなくCSSでプロジェクトを設定するようになった。  
`tailwind.config.js`ファイルの代わりに、TailwindをインポートするCSSファイル内で直接カスタマイズを記述できる。

```css
@import "tailwindcss";

@theme {
  --font-display: "Satoshi", "sans-serif";
  --breakpoint-3xl: 1920px;
  --color-avocado-100: oklch(0.99 0 0);
  --color-avocado-200: oklch(0.98 0.04 113.22);
  --color-avocado-300: oklch(0.94 0.11 115.03);
  --color-avocado-400: oklch(0.92 0.19 114.08);
  --color-avocado-500: oklch(0.84 0.18 117.33);
  --color-avocado-600: oklch(0.53 0.12 118.34);
  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
  --ease-snappy: cubic-bezier(0.2, 0, 0, 1);
  /* ... */
}
```

この新しいCSSファースト設定では、デザイントークンの設定、カスタムユーティリティやバリアントの定義など、`tailwind.config.js`ファイルで行っていたほとんどすべての設定を行うことができる。  
これにより、プロジェクト内のファイル数が減り、管理が容易になる。  
また、CSS変数で定義するようになったので、CSSやJavaScript側から扱うことが容易になった。

## CSSテーマ変数
Tailwind CSS v4.0では、すべてのデザイントークンがデフォルトでCSS変数として利用できるようになったので、CSSを使って実行時に必要な値を参照することができるようになった。  

先ほどの`@theme`の例では、すべての値が通常のカスタムプロパティとしてCSSに追加される。
```css
:root {
  --font-display: "Satoshi", "sans-serif";
  --breakpoint-3xl: 1920px;
  --color-avocado-100: oklch(0.99 0 0);
  --color-avocado-200: oklch(0.98 0.04 113.22);
  --color-avocado-300: oklch(0.94 0.11 115.03);
  --color-avocado-400: oklch(0.92 0.19 114.08);
  --color-avocado-500: oklch(0.84 0.18 117.33);
  --color-avocado-600: oklch(0.53 0.12 118.34);
  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
  --ease-snappy: cubic-bezier(0.2, 0, 0, 1);
  /* ... */
}
```
これにより、インラインスタイルとしてこれらの値を再利用したり、Motionなどのライブラリに渡してアニメーション化したりすることが容易になる。

## ダイナミック・ユーティリティ値とバリアント
Tailwind CSS v4.0では、ユーティリティやバリアントの動作が簡素化され、設定や任意値構文（`mt-[18px]`みたいなやつ）を使用せずに、特定の種類の任意値を受け入れることができるようになった。  
（個人的には書きやすくはなるけど、秩序としては崩れやすいのであまり好きではない。。。）  

例えば、v4.0では、設定なしで任意のサイズのグリッドを作成できる。
```html
<div class="grid grid-cols-15">
  </div>
```
また、カスタムの真偽値データ属性を定義せずにターゲットにすることもできる。
```html
<div data-current class="opacity-75 data-current:opacity-100">
  </div>
```
`px-*`、`mt-*`、`w-*`、`h-*`などのスペーシングユーティリティも、単一のスペーシングスケール変数から動的に派生し、設定なしで任意の値を受け入れるようになった。
```css
@layer theme {
  :root {
    --spacing: 0.25rem;
  }
}

@layer utilities {
  .mt-8 {
    margin-top: calc(var(--spacing) * 8);
  }
  .w-17 {
    width: calc(var(--spacing) * 17);
  }
  .pr-29 {
    padding-right: calc(var(--spacing) * 29);
  }
}
```
v4.0と同時にリリースされたアップグレードツールは、不要になった任意値を使用している場合、これらのユーティリティのほとんどを自動的に簡素化してくれる。

## 近代化されたP3のカラーパレット
Tailwind CSS v4.0では、デフォルトのカラーパレットが`rgb`から`oklch`にアップグレードされた。  
`oklch`はより広い色域を持つため、sRGB色空間では表現できなかった鮮やかな色を表現できるようになった。  

v3とのバランスを維持するように調整されているため、既存のプロジェクトをアップグレードしても大きな変更は感じられないとのこと。

## コンテナクエリ対応
Tailwind CSS v4.0では、コンテナクエリがコア機能として組み込まれ、`@tailwindcss/container-queries`プラグインは不要になった。
```html
<div class="@container">
  <div class="grid grid-cols-1 @sm:grid-cols-3 @lg:grid-cols-4">
    </div>
</div>
```
また、新しい`@max-*`バリアントを使用して、`max-width`コンテナクエリもサポートされるようになった。
```html
<div class="@container">
  <div class="grid grid-cols-3 @max-md:grid-cols-1">
    </div>
</div>
```
通常のブレークポイントバリアントと同様に、`@min-*`と`@max-*`バリアントをスタックして、コンテナクエリの範囲を定義することもできる。
```html
<div class="@container">
  <div class="flex @min-md:@max-xl:hidden">
    </div>
</div>
```

## 新しい3D transformユーティリティ
`rotate-x-*`、`rotate-y-*`、`scale-z-*`、`translate-z-*`など、3D変換を行うためのAPIがついに追加された。
```html
<div class="perspective-distant">
  <article class="rotate-x-51 rotate-z-43 transform-3d ...">
    </article>
</div>
```

## 拡張されたグラデーションAPI
新しいグラデーション機能がたくさん追加され、カスタムCSSを記述することなく、よりファンシーなエフェクトを引き出せるようになった。

### 線形勾配角度
線形グラデーションで角度を値として指定できるようになった。  
`bg-linear-45`のようなユーティリティを使用して、45度の角度でグラデーションを作成できる。
```html
<div class="bg-linear-45 from-indigo-500 via-purple-500 to-pink-500"></div>
```
また、`bg-gradient-*`は`bg-linear-*`に名前が変更された。

### 勾配補間モディファイア
グラデーションの色補間モードを修飾子で制御できるようになった。  
例えば、`bg-linear-to-r/srgb`クラスはsRGBで補間し、`bg-linear-to-r/oklch`はOKLCHで補間する。
```html
<div class="bg-linear-to-r/srgb from-indigo-500 to-teal-400">...</div>
<div class="bg-linear-to-r/oklch from-indigo-500 to-teal-400">...</div>
```
OKLCHやHSLのような極座標色空間を使用すると、`from-`*と`to-*`の色がカラーホイール上で離れている場合、より鮮やかなグラデーションを作成できる。  
v4.0ではデフォルトでOKLABが使用されているが、これらの修飾子を追加することで、異なる色空間を使用して補間することもできる。

### 円錐勾配と放射状勾配
円錐グラデーションと放射状グラデーションを作成するための新しい`bg-conic-*`および`bg-radial-*`ユーティリティが追加された。
```html
<div class="size-24 rounded-full bg-conic/[in_hsl_longer_hue] from-red-600 to-red-600"></div>
<div class="size-24 rounded-full bg-radial-[at_25%_25%] from-white to-zinc-900 to-75%"></div>
```
これらの新しいユーティリティは、既存の`from-*`、`via-*`、`to-*`ユーティリティと連携して動作し、線形グラデーションと同じ方法で円錐グラデーションと放射状グラデーションを作成できる。  
また、色補間方法を設定するための修飾子や、グラデーションの位置などの詳細を制御するための任意値サポートも含まれている。

## @starting-styleのサポート
新しい`starting`バリアントは、CSSの新しい`@starting-style`機能をサポートし、要素が最初に表示されたときの要素プロパティの遷移を可能する。
```html
<div>
  <button popovertarget="my-popover">Check for updates</button>
  <div popover id="my-popover" class="transition-discrete starting:open:opacity-0 ...">
    </div>
</div>
```
`@starting-style`を使用すると、JavaScriptを一切使用せずに、ページに表示される要素にアニメーションを適用できる。  
ただ、ブラウザのサポートはまだ十分ではないので注意。

## not-* variant
CSSの`:not()`疑似クラスをサポートする新しい`not-*`バリアントが追加された。
```html
<div class="not-hover:opacity-75">
  </div>
```
これは以下のようにCSSに変換される。
```css
.not-hover\:opacity-75:not(*:hover) {
  opacity: 75%;
}
@media not (hover: hover) {
  .not-hover\:opacity-75 {
    opacity: 75%;
  }
}
```
また、メディアクエリや`@supports`クエリを否定することもできる。
```html
<div class="not-supports-hanging-punctuation:px-4">
  </div>
```
これは以下のようにCSSに変換される。
```css
.not-supports-hanging-punctuation\:px-4 {
  @supports not (hanging-punctuation: var(--tw)) {
    padding-inline: calc(var(--spacing) * 4);
  }
}
```

## まとめ
様々な変化がありましたが、やっぱりJavaScriptではなくCSSでプロジェクトを設定するようになったのが、一番大きいですね。  
個人的にはarbitrary valuesが簡素化されたのは、予め設定されたデザインシステムに則ったユーティリティクラスが用意されているTailwind CSSとしての秩序が崩れやすいのでこの変更点はあまり好きではないですが、これも変化として受け入れようと思います。

## 参考
https://tailwindcss.com/blog/tailwindcss-v4
https://tailwindcss.com/
