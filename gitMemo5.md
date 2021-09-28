■作業を一時避難する
commitされていない変更がある場合ブランチの移動はできない。
ので、git stashすることでワークツリーとstageにある変更分を
別の場所に移動させる。
```
git stash 
git stash save // 上記と同じ
```
■stashを確認
```
git stash list
```

■stashを復元
```
//ワークツリーのみ
git stash apply
// ワークツリーとstage
git stash apply --index
// 特定のstashを復元
git stash apply stash@{0} // 0が最新
```

■stashの削除
```
//最新
git stash drop
//指定
git stash drop stash@{1}
//すべて
git stash clear
```