---
title: 論文 Exploration by Random Network Distillation
date: 2018-11-10T18:15:40+09:00
lastmod: 2018-11-10T18:15:40+09:00
draft: false
cover: "/images/2018/11/progress-rnd.svg"
categories: ["論文読み"]
tags: ["Reinforcement Learning"]
description: 
---

Montezuma’s RevengeというDeep RLで従来クリアが難しいとされていたゲームのスコアを大幅に更新したOpenAIの研究。

* OpenAIのブログはこちら
https://blog.openai.com/reinforcement-learning-with-prediction-based-rewards/
* 論文はこちら https://arxiv.org/abs/1810.12894
* 実装も公開されている https://github.com/openai/random-network-distillation

どのくらいスコアを更新したかというとこのくらい。

![RLProgress](/images/2018/11/progress-rnd.svg)

本研究で学習するnetworkとしては内発的な報酬を決めるためのnetworkと方策を決めるためのnetworkがある。
この2つの組み合わせで探索したことの無い状態を見つけ出すように行動し、同時に高い報酬も得るようになる。

## 内発的な報酬を決めるnetworkの学習
観測から潜在変数を推定する、ある関数$f(x)$があるとする。
これはNeural Networkで構築されており、これをtarget networkと呼ぶ。
target networkが正しく潜在表現を抽出できていると仮定するならば、推定関数$\hat{f}(x;\theta)$は以下の最小2乗誤差で評価できる。

\begin{equation}
\|\hat{f}(x;\theta) - f(x)\|^2
\end{equation}

仮定したtarget networkであるが、そんなものが分かるわけがないので、本研究ではパラメタをランダムに初期化したneural networkをtarget networkとしている。
この誤差を最小化する過程が、論文タイトルにある、Random Network　Distillation(ランダムなネットワークに対する蒸留)なのである。
しかし本当に潜在変数を求めたいのであれば、Random Network　Distillationには何の意味も無いように思える。
教師あり学習でなく、VAEなどの教師なし学習の手法が使われるだろう。
ではなぜ、Distillationをするかというと、その誤差が"探索ボーナス"として使えるからである。
経験したことがない観測かどうかを評価したいときに、従来、状態への到達数のカウントなどが用いられたが、Distillationの誤差を使えばそれと同じような効果が得られるのである。

潜在変数の推定networkの更新は方策networkの更新と同時に行われる。
それによって、行動による経験の度合いとして対応付けることができる。

## 方策networkの学習
方策networkの学習にはPPOが用いられた。

報酬としては内発的な報酬
\begin{equation}
i_t = \|\hat{f}(s\_{t+1}) - f(s\_{t+1})\|^2
\end{equation}
とゲームから得られる通常の外発的な報酬$e_t$が用いられる。

この報酬を一定のホライズンについて記録して累積報酬$R\_{I, i}$、$R\_{E, i}$、そしてアドバンテージ$A\_{I, i}$、$A\_{E, i}$を求める。
これがPPOの過程で用いられる。

ただし、潜在変数推定networkはゲームの盤面が変わっても同じものが用いられる。
そのときに、networkの出力はゲームの盤面によってスケールが変わってしまうので、normalizationが必要になる。
target networkと推定networkに入力される観測について平均値のバイアスを引くのと一定の値域に収まるようにscalingすることによって潜在変数networkのnormalizationとしている。

# 動画
<iframe width="560" height="315" src="https://www.youtube.com/embed/40VZeFppDEM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>