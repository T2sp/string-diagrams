この記事は，TikZでモノイダル圏のストリング図式を描く方法の自分用備忘録である．

# 必要なTikZライブラリ

```TeX
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

`\coordinate, \node, \draw` などのコマンドの細かいオプションをいちいち指定するのは面倒なので，プリアンブルなどに `\tikzset{中身}` を書いてよく使うオプションの設定に名前をつけて保存しておくと良い．以下では  `\tikzset{中身}` の `中身` として有用そうな例をいくつか紹介する．

## 線の中間の矢印

```TeX
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
引数には矢印の位置を代入する．

```TeX:使用例
\begin{tikzpicture}
    \path coordinate (s)
    ++(1,0) coordinate (t)
    ;
    \draw[->-=.5] (s) -- (t);
\end{tikzpicture}
```

矢印のスタイルが気に入らない場合は `\arrow{...}` の部分を変更すれば良い．矢印の細かいサイズ調整については [公式ドキュメント](https://tikz.dev/tikz-arrows) を参照．

## 点

```TeX
bullet/.style={
    circle,fill,
    inner sep=2pt,
    label={#1}
}
```
ストリング図式としては，モノイダル圏の射を表す．

```TeX:使用例
\begin{tikzpicture}
    \path coordinate (s)
    ++(0,1) coordinate[bullet,label=left:$f$] (f)
    ++(0,1) coordinate (t)
    ;
    \draw[->-=.25,->-=.75] (s) -- node[midway,right] {$x$} (f) -- node[midway,right] {$y$} (t);
\end{tikzpicture}
```

## 箱

```TeX
squarednode/.style={
    rectangle, draw=black!60, minimum size=5mm
}
```
モノイダル圏の射を，点ではなく四角の箱で描くこともある．この記事では点で描く流儀を採用するが，参考までに載せておく．

```TeX:使用例
\begin{tikzpicture}
    \path coordinate (s)
    ++(0,1) coordinate[squarednode,label=center:$f$] (f)
    ++(0,1) coordinate (t)
    ;
    \draw[->-=.25,->-=.75] (s) -- node[midway,right] {$x$} (f) -- node[midway,right] {$y$} (t);
\end{tikzpicture}
```

# TikZの基礎

TikZの図は，座標点と，それらの間を繋ぐ曲線から構成されている．
`\path ... ;` の構文が基本だが，略記として座標指定に特化した `\coordinate ... ;`，頂点の描画に特化した `\node ... ;` や曲線の描画に特化した `\draw ... ;` コマンドが用意されている．
**構文文末のセミコロン `;` を忘れてはいけない**

## 座標の指定方法

原則 `coordinate[option] (名前) at (座標)` を用いる．`(座標)` はデカルト座標と極座標の2通りで指定できる．
オプションで描画スタイルやラベルを指定することができる．

点が少ない場合は絶対座標でも良いが，点が多い場合は相対座標を使うと便利だと思う．

