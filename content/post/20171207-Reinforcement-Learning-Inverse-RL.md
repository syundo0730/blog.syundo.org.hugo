---
title: "強化学習についてまとめる(7) 逆強化学習"
date: 2017-12-07T22:10:44+09:00
categories: ["まとめ"]
tags: ["Reinforcement Learning"]
draft: true
---

ここまでは、報酬というのは何らかの形で与えられるものとしてきた。
しかし、実際に扱いたい問題に対して、目標状態や終端状態にだけ定義された報酬を使うだけでは学習が難しいことがある。
ゴールに到達することでしか報酬を得られない場合、ゴールに到達する確率が低いと学習が進まないことが容易に想像できる。
あるいはペットボトルを傾けてコップに水を入れる問題など、初期状態と終端状態だけでなくそのすべての過程が適切に評価されないと実現できない問題もあるだろう。

逆強化学習は、単純には報酬が導き出せない場合のために、報酬を設計する問題を解く(報酬設計)手法である。
