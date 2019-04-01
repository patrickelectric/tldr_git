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

```shell
#!/bin/sh

echo "Running pre-push hook !"
git fetch origin
git rebase origin/master
```

# `commit-msg` example:
```python
#!/usr/bin/env python
# Add markdown information about issue

import os
import re
import sys

print("Running commit-msg hook !")
url = os.popen("git ls-remote --get-url").read().rstrip() + "/issues/"

commit_msg = open(sys.argv[1]).read()
issue_regex = re.compile(r"#\d+")

for result in re.findall(issue_regex, commit_msg):
    mk = '[%s](%s)' % (result, url + result[1:])
    print("Processing:", result)
    commit_msg = commit_msg.replace(result, mk)

f = open(sys.argv[1], 'w')
f.write(commit_msg)
f.close()

exit(0)
```
