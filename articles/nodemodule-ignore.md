---
title: "誤ってpushしたnode_modulesをGitHubから削除する"
emoji: "😧"
type: "tech"
topics: ["webpack", "GitHub", "Git", "初心者"]
published: true
---

環境構築して GitHub に上げた際に「なんか重いな」と思ったら、node_modules を普通にアップしちゃってました。。。やっちまった。。。

なので、GitHub にアップしてしまった node_modules を削除する方法を備忘として残しておきます。

# 1 .gitignore ファイル作成

エディタ上で.gitignore ファイル作成するか、以下のようにターミナルで空の新規ファイルを作成します。

```
$ touch .gitignore
```

# 2 .gitignore に node_modules を記述

.gitignore ファイルに以下のように node_modules を記述します。

```
# dependencies
/node_modules
```

# 3. ローカルに残しつつ node_modules を Git の管理対象から外す

ターミナルから以下のコマンドで node_modules を Git の管理対象から外します。

```
$ git rm -r --cached node_modules
```

:::message
`--cached`オプションを付けることで、ローカルに node_modules を残したまま管理対象から外すことができます。
:::

あとは、node_modules が管理対象から外れたことで差分が発生しているのため、それをコミット&プッシュして完了です。

## 参考

https://qiita.com/ytkt/items/a2afd6be8e4f06c1ea25
https://qiita.com/y_sone/items/2723bb7c1b63abb28f57
