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

- Command: `wandbox-compile-file filename`

  指定したファイルをコンパイルします。
  呼び出すコンパイラは拡張子から自動判別されます。

- Command: `wandbox-compile-region from to`

  指定したリージョンをコンパイルします。

- Command: `wandbox-compile-buffer`

  現在のバッファをコンパイルします。

- Function: `wandbox-compile &rest profile &key compiler options code stdin compiler-option runtime-option name file`

  コンパイラやオプションなどを直接指定してコンパイルします。
  引数 compiler, options, code, stdin, compiler-option, runtime-option は
  Wandbox API に渡すための文字列パラメータを指定します。
  compiler-option, runtime-option はリスト形式で指定することもできます。

  また追加機能として、ファイル名やプロファイル名の指定ができます。
  プロファイルについては `wandbox-profiles` を参照してください。

- Variable: `wandbox-profiles`

  Wandbox API の呼び出しパラメータを設定するためのプロファイル (という名のテンプレート) 群です。
  ユーザ独自のプロファイルを追加することもできます。


## Example

```elisp
;; コンパイラ、オプションを指定してコンパイルする
(wandbox :compiler "gcc-head" :options "warning" :code "main(){}")
```

```elisp
コードを C 言語としてコンパイルする (プロファイルを利用)
(wandbox-compile :name "C" :code "main(){}")
```

```elisp
;; 指定したファイルをコンパイルする
(wandbox-compile :name "C" :file "/path/to/prog.c")
```

```elisp
;; 標準入力を利用する
(wandbox-compile :name "Perl" :code "while (<>) { print uc($_); }" :stdin "hello")
```

```elisp
;; 独自プロファイルを追加する
(add-to-list 'wandbox-profiles '(:name "ANSI C" :compiler "clang-head" :options "warning,c89"))
(wandbox-compile :name "ANSI C" :file "/path/to/prog.c")
```


## TODO

- [ ] add merge-plist
- [ ] コンパイル結果のデータをユーザが弄れるようにする
- [ ] gist などのコード片を扱えるようにする
- [ ] 複数プロファイルの指定
- [ ] コンパイラの設定を簡単にしたい
- [ ] request.el を利用する (ただし依存関係が増える)


## License

このソフトウェアは MIT ライセンスのもとで公開されています。
