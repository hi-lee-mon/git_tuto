■リベース
mergeじゃない変更の取り込み
mergeと違って親コミットが1つになるため、履歴が整理された状態になる
```
*main
 feature
のときmainにfeatureの内容を取り込みたい場合
git checkout feature
↓
git rebase master // featureブランチで実行
↓
featureを指すコミットが削除されてreabaseでできたコミットを指すようになる。
rebaseコミットはmasterのコミットの親コミットとして作成される。
↓
git checkout main
↓
git merge feature
↓
fast forwardする。
```
※rebaseしたブランチの親コミットが変更されるからrebaseと呼ぶa
