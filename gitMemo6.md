■リベース
mergeじゃない変更の取り込み
mergeと違って親コミットが1つになるため、履歴が整理された状態になる。
rebase時に指定したコミットを取り込み、指定したコミットを親コミットとして新たなコミットを作成する。必ず指定されたコミットはfase fowardすること。
```
*main
 feature
のときmainにfeatureの内容を取り込みたい場合
git checkout feature
↓
git rebase master // featureブランチで実行
↓
featureブランチが指すコミットが削除されてreabaseでできたコミットを指すようになる。
rebaseコミットはmasterのコミットを親コミットとして作成される。
↓
git checkout main //mainブランチへ移動
↓
git merge feature //fast forwardする。
↓
リベース完
```
※rebaseしたブランチの親コミットが変更されるからrebaseと呼ぶ。

