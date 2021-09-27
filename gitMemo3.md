■ワークツリーにあるリモートリポジトリの状態を確認する
```
git remote // リポジトリの確認
git remote -v // urlの確認
```

■リモートの情報を詳しく調べる
```
git remote show <リモート名>
```

■リモートリポジトリの追加
リモートリポジトリは複数登録することができる。
gitHubでリポジトリを作成して、ローカルのgit initした場所で行うと実現できる。
```
git remote add リモート名 リモートURL
git remote add origin https://~
```
上記を行うことで、ローカルからリモートリポジトリへ<リモート名>を利用してアクセスできるようになる。簡単にpushやfetchをするためにおこなう。

■リモートから情報を取得する方法（fetch）
リモートリポジトリのデータを取ってきて、ローカルリポジトリのリモートブランチに保存する。
```
git fetch <リモート名>
git fetch origin
// 以下オプション
-p リモーリポジトリから削除されているブランチをローカルでも削除する
```
```
// リモートブランチの確認
git branch -a
```
リモートブランチのデータをワークツリーに反映させる。
```
git merge リモートブランチ
git merge remotes/origin/main
以下省略記法
git merge origin/main
```

■リモートから情報を取得する方法（fetch）
git pull リモート名 ブランチ名
※現在いるブランチにmergeされるため注意

■リモート名の変更
```
git remote rename 旧 新
```

