# master ブランチから main ブランチに変更
以下で変更可能
`git branch -m master main`

参考：git branch オプション
```
Specific git-branch actions:
    -a, --all             list both remote-tracking and local branches
    -d, --delete          delete fully merged branch
    -D                    delete branch (even if not merged)
    -m, --move            move/rename a branch and its reflog
    -M                    move/rename a branch, even if target exists
    etc...
```

## デフォルト main ブランチにする
以下のコマンドを実行することで変更できる
`git config --global init.defaultBranch main`

- [参考](https://qiita.com/fk_chang/items/a4839a595fef9a2c3724)

