■データのバージョンを管理する方法
ワークツリー⇒ローカルリポジトリ⇒リモートリポジトリ
※リポジトリとはデータの置き場のこと。

■他の人の変更を取り込む方法
リモートリポジトリ⇒ローカルリポジトリ⇒ワークツリー


■ローカルは3つのエリアに分かれる
ワークツリー / ステージ / ローカルリポジトリ

ワークツリーは作業場
ステージはコミットの準備をする場所
ローカルリポジトリはスナップショットを記録する

■Gitのデータ構造（1周目）
[git add]
ワークツリーにあるindex.htmlを一旦リモートリポジトリに移動させる。
リモートリポジトリにてindex.htmlを圧縮したファイルである圧縮ファイルAを作成する（実際のファイル名はファイルの内容をハッシュ関数で暗号化したもの）。
次にステージにインデックスというファイルを作成する。インデックスには、ファイル名index.htmlは圧縮ファイルAのものであるというマッピング情報と、index.htmlはプロジェクトでのどの位置にあるのかというディレクトリ構造が記録される。

[git commit]
リモートリポジトリにはステージにあるインデックスが記録しているディレクトリ構造を記録したツリー1というファイルを作成する。
次にコミット1というファイルを作成する。コミット1には、ツリー１の作成者やメールアドレス、日付、コミットメッセージが記録される。

[データ整理]
・ワークツリー
編集しているファイル
・ステージ
インデックスファイル
・ローカルリポジトリ
圧縮ファイル / ツリーファイル / コミットファイル

■Gitのデータ構造（2周目）
[git add]
ワークツリーにあるcss/home.cssを一旦リモートリポジトリに移動させる。
リモートリポジトリにてhome.cssを圧縮したファイルである圧縮ファイルBを作成する。
次にステージにあるインデックスに、ファイル名home.cssは圧縮ファイルBのものであるというマッピング情報と、home.htmlはプロジェクトでのどの位置にあるのかというディレクトリ構造（cssディレクトリの下に存在すること）が記録される。

[git commit]
リモートリポジトリにはステージにあるインデックスが記録しているディレクトリ構造を記録したツリー2というファイルを作成する（ステージをスナップショットで記録？）。
次にコミット2というファイルを作成する。コミット2には、直前のコミット名を記録する。またツリー2の作成者やメールアドレス、日付、コミットメッセージが記録される。
バージョンを辿る際は、ローカルリポジトリに記録されている直前のコミットを辿ることで遡る。

[データ整理]
・ワークツリー
編集しているファイル
・ステージ
インデックスファイル（index.htmlと圧縮ファイルAのマッピング & home.cssと圧縮ファイルBのマッピング & ディレクトリ構造）
・ローカルリポジトリ
圧縮ファイルA & B / ツリーファイル / コミットファイル


■Gitのデータ構造（3周目）
[git add]
ワークツリーにあるindex.htmlを変更する。
変更のあったindex.htmlのみをローカルリポジトリに移動
リモートリポジトリにてindex.htmlを圧縮したファイルである圧縮ファイルCを作成する。
次にステージにあるインデックスに、ファイル名index.htmlは圧縮ファイルAであるというマッピング情報を圧縮ファイルCのものであるというマッピング情報に書き換える。またhome.htmlはプロジェクトでのどの位置にあるのかというディレクトリ構造が記録される。

[git commit]
リモートリポジトリにはステージにあるインデックスが記録しているディレクトリ構造を記録したツリー3というファイルを作成する。
次にコミット3というファイルを作成する。コミット3には、直前のコミット名を記録する。またツリー3の作成者やメールアドレス、日付、コミットメッセージが記録される。

[データ整理]
・ワークツリー
編集しているファイル
・ステージ
インデックスファイル（ファイルと圧縮ファイルのマッピング情報 & ディレクトリ構造）
・ローカルリポジトリ
圧縮ファイルA & B & C / ツリーファイル / コミットファイル


●なぜインデックスとツリーファイルがあるのか
インデックスとツリーファイルの内容は全く同じ。
ではなぜ、分かれているのか？
それは整理されたファイルだけをバージョン管理したい、一部だけコミットしたいから。
現在のGitのバージョン管理では、ステージにあるインデックスというファイルに上書き保存していって、本当にコミットしたい内容だけツリーファイルを作成している。これを直接ツリーファイルを作成すると、1部分だけコミットするということが出来なくなるし、整理されていないファイルがツリーファイルとなる。

●なぜツリーファイルとコミットファイルがわかれているのか？
ツリーファイルはディレクトリごとに作成されることが原因。
src/components/atoms/Button.tsxだったら
componentsツリーファイルとatomsツリーファイルが作成される。
コミットファイルはツリーファイルのなかでもルートのディレクトリに対して作成される。もし分かれずに作成されていると、すべてのツリーファイルに対してコミットファイルができてしまい非効率。
では、なぜツリーファイルごとに作成されているのかというと、gitのファイル容量を軽くするため。例えば一部分のファイルが変更されたときに、すべてのディレクトリ構造を1つのツリーファイルで管理していた場合は、そのツリーファイルを複製して作り返る必要がある。
実際はツリーファイルを分けているため、1部分の変更があれば変更のあったツリーファイルだけ修正すればよいので軽くなる。

■Gitオブジェクトについて
圧縮ファイルやツリーファイル、コミットファイルをGitオブジェクトという。Gitオブジェクトは.git/objectsに保存されている。

●圧縮ファイル
圧縮ファイルはファイルの中身そのものを圧縮したもの。
実体はBlobオブジェクト。
ファイル名はハッシュ関数により作成されたハッシュIDになっている。ハッシュIDはファイルの内容から作成されているので、ファイルに変更があればハッシュIDに変化がある。ファイルの内容に変化がない限り、ハッシュIDが変化することがない。ハッシュIDが変化した、つまりファイルの内容が変わったことになるため、その場合は新たな圧縮ファイルを作成してステージのインデックスファイルに上書き保存を行う。
※圧縮ファイルはファイル名に関心を持っていないため、中身が一緒でファイル名だけ異なるファイルがあると、圧縮ファイル名・ハッシュIDは同じになる。

●ツリーファイル
ツリーファイルは構造や名前を持たない圧縮ファイルに構造を与えるためのもので、圧縮ファイルやツリーファイルを保存しているのです。
圧縮ファイルのファイル名はハッシュIDなので、ファイル名とファイルの中身の内容が繋がっていません。例えばindex.htmlはハッシュIDxxxのものである、というようなファイル名とファイルの中身の組み合わせで保存するためのものがツリーファイルです。正確にはツリーオブジェクトといいます。

またツリーファイルは１つのディレクトリに対して１つ作成される。
git cat-file -p <Gitオブジェクト名>でGitオブジェクトの中身を確認できる。
```
// 利用したコマンド
mkdir test
echo "hello" > test/hello.txt
git add test
git commit -m "add test dir"
// 最新のコミットファイル(HEAD)を確認。
//コミットファイルにはツリーファイルの名前が書いてある
git cat-file -p HEAD 
git cat-file -p <Gitオブジェクト名（最新のツリーファイル名）>
```
```
/src/components/atoms/Button.tsxのとき
ツリーファイルの中にsrcツリーファイルがあり。
srcツリーファイルの中にcomponentsツリーファイルがあり
componentsツリーファイルの中にatomsツリーファイルがあり
atomsツリーファイルの中にButton.tsxという圧縮ファイル（blob）が存在する
```
```
// ツリーファイルの中身
// 圧縮ファイル + ファイル名
100644 blob 9daeafb9864cf43055ae93beb0afd6c7d144bfa4  test.txt
// ツリーファイル + ディレクトリ名
040000 tree aaa96ced2d9a1c8e72c56b253a0e2fe78393feb7  test
```
ステージにあるインデックスファイルは上記ツリーファイルと同じような構成になっている。

●コミットファイル
中身は以下の通り
```
$ git cat-file -p HEAD
tree bf06de64ed69b31018a8d87128a3e769a13aef51  //ルートのツリーファイル
parent e7f349f945d21c26e018a5e45d7e1a3fec740ccd  //直前のコミットファイル
author hi-lee-mon <shunmel.09@gmail.com> 1632664384 +0900  // 作成者
committer hi-lee-mon <shunmel.09@gmail.com> 1632664384 +0900

gitメモ.mdを追加  //コミットメッセージ
```

◎まとめ
Gitを使ったローカルの操作は、上記3つのファイルに対して操作しているだけ。