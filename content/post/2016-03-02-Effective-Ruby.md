+++
date = "2016-03-02T00:17:58+09:00"
draft = true
title = "Effective Ruby 読書メモ"

+++

# Effective Ruby
！！！このメモは結構書籍に書いてあることそのままをメモっているので、公開してはいけない！！！

## Ruby に身体を慣らす

### Ruby は何を真と考えているかを正確に理解しよう
false と nil 以外は真である

### オブジェクトを扱うときにはnilかもしれないということを忘れないようにしよう

* nil? メソッドはレシーバがnilならtrue、そうでなければfalseを返す
* to_s, to_iなどのメソッドを使ってnilオブジェクトを強制的に型変換しよう
* Array#compactメソッドは、レシーバーのコピーからすべてのnil要素を取り除いた形のものを返す

### Rubyの暗号めいたPerl風機能を避けよう
Perlから影響を受けた機能は使わないほうが読みやすいという話。言われなくても使わない。

### 定数がミュータブルなことに注意しよう
定数は、名前が大文字の英字と数字、アンダースコアで構成される識別子

定数への参照は固定されるが、その実体のイミュータブルさは保証されていないという話。
.freezeメソッドを呼んでやる必要がある。

### 実行時の警告に注意しよう
-w オプションを付ければコンパイル時のエラーも表示される
