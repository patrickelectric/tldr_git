# Git hooks are in `.git/hooks`
They can be:
- applypatch-msg
- commit-msg
- post-update
- pre-applypatch
- pre-commit
- pre-push
- pre-rebase
- pre-receive
- prepare-commit-msg
- update
 
# `pre-push` example:
```shell
#!/bin/sh

echo "Running pre push hook!"
repository_path=$(git rev-parse --git-dir)/..
# Run testcomments script to check if all comments are valid before pushing it to the remote
$repository_path/tools/testcomments.sh
```

```
#!/bin/sh

echo "Running pre-push hook !"
git fetch origin
git rebase origin/master
```
