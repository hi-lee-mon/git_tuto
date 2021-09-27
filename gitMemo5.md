■作業を一時避難する
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