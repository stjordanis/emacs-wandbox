Wandbox for Emacser
===================

Wandbox API を利用するための Emacs 拡張ライブラリ。


## Wandox とは

Wandbox は @melponn と @kikairoya が開発したオンラインコンパイラです。
主に C++ に特化している他、C, Perl, Python, Ruby, PHP, Common Lisp など
様々な言語に対応しています。

* [Wandbox Home](http://melpon.org/wandbox/)
* [API Reference](https://github.com/melpon/wandbox/blob/master/kennel/API.rst)


## 使い方

wandbox.el を拾ってきてロードして下さい。

適当なファイルを開いて M-x `wandbox-compile-buffer` を実行すると
実行結果が Wandbox Output バッファに表示されます。

より詳細なオプションを指定したい場合は `wandbox-compile` を利用してください。


## リファレンス

- Command: `wandbox-compile-file (filename)`

  指定したファイルをコンパイルします。
  呼び出すコンパイラは拡張子から自動判別されます。

- Command: `wandbox-compile-region (from to)`

  指定したリージョンをコンパイルします。

- Command: `wandbox-compile-buffer`

  現在のバッファをコンパイルします。

- Function: `wandbox-compile (&rest profile &key compiler options code stdin compiler-option runtime-option lang name file save)`

  コンパイラやオプションなどを直接指定してコンパイルします。
  `wandbox` は `wandbox-compile` のエイリアスです。
  引数 `compiler`, `options`, `code`, `stdin`, `compiler-option`, `runtime-option` は
  Wandbox API に渡すための文字列パラメータを指定します。
  `compiler-option`, `runtime-option` はリスト形式で指定することもできます。

  また追加機能としてファイル名やプロファイル名の指定ができます。
  プロファイルについては `wandbox-profiles` を参照してください。

- Variable: `wandbox-profiles`

  Wandbox API の呼び出しパラメータを予め設定しておくためのプロファイル (という名のテンプレート) 群です。
  ユーザ独自のプロファイルを追加することもできます。

- Variable: `wandbox-precompiled-hook`

  ここに関数を追加しておくことで、Wandbox を呼び出す前のプロファイルを編集できます。
  呼び出された関数の返り値はプロファイルにマージされます。


## Example

```elisp
;; コンパイラとオプションを指定してコンパイルする
(wandbox :compiler "gcc-head" :options "warning" :code "main(){}")
```

```elisp
コードを C 言語としてコンパイルする (プロファイルを利用)
(wandbox :lang "C" :code "main(){}")
```

```elisp
;; 指定したファイルをコンパイルして、パーマリンクを生成する
(wandbox :lang "C" :file "/path/to/prog.c" :save t)
```

```elisp
;; 標準入力を利用する
(wandbox :lang "Perl" :code "while (<>) { print uc($_); }" :stdin "hello")
```

```elisp
;; 独自プロファイルを追加する
(add-to-list 'wandbox-profiles '(:name "ANSI C" :compiler "clang-head" :options "warning,c89"))
(wandbox :name "ANSI C" :file "/path/to/prog.c")
```

```elisp
;; gist のコード片をコンパイルする
;; sample: https://gist.github.com/219882
(wandbox :name "CLISP" :gist 219882 :stdin "Uryyb Jbeyq!")
```

```elisp
;; ファイルの代わりにURLを指定してコンパイルする (:url キーワードの追加)
(defun* wandbox-option-url (&key url &allow-other-keys)
  (if url (plist-put nil :code (wandbox-fetch url))))
(add-to-list 'wandbox-precompiled-hook #'wandbox-option-url)

(wandbox :lang "C" :url "http://localhost/prog.c")
```


## TODO

- [ ] [Done] add merge-plist
- [ ] コンパイル結果のデータをユーザが弄れるようにする
- [ ] [Done] gist などのコード片を扱えるようにする
- [ ] 複数プロファイルの指定
- [ ] コンパイラの設定を簡単にしたい
- [ ] request.el を利用する (ただし依存関係が増える)
- [ ] /wandbox/api/list.json を利用したプロファイルの自動生成
      API 応答時間が遅いのを何とかするのと
      コンパイル時の定数展開ができればなお良い
- [ ] テストの作成


## License

このソフトウェアは MIT ライセンスのもとで公開されています。
