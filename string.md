この記事は，TikZでモノイダル圏のストリング図式を描く方法の自分用備忘録である．

# 必要なTikZライブラリ

```
\usepackage{tikz}
\usetikzlibrary{
    intersections,
    decorations,
    calc,
    patterns,
    through,
    positioning,
    arrows,
    backgrounds,
    cd,
    spath3,
    knots
}
```

# よく使うtikzset

`\coordinate` や `\draw` コマンドの細かいオプションをいちいち指定するのは面倒なので，プリアンブルなどに `\tikzset{中身}` を書いてよく使うオプションの設定に名前をつけて保存しておくと良い．以下では  `\tikzset{中身}` の `中身` として有用そうな例をいくつか紹介する．

## 線の中間の矢印

```
->-/.style={
        decoration={
            markings,
            mark=at position #1 with {\arrow{>}}
        },
        postaction={decorate}
}
-<-/.style={
    decoration={
        markings,
        mark=at position #1 with {\arrow{<}}
    },
    postaction={decorate}
}
```

矢印のスタイルが気に入らない場合は `\arrow{...}` の部分を変更すれば良い．矢印の細かいサイズ調整については [公式ドキュメント](https://tikz.dev/tikz-arrows) を参照．

