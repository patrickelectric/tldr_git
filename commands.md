# Status
## Show ignored files
`git status --ignored`

# Help
## Show help
`git help command`

# Grep
## Grep only tracked files
`git grep -e "pattern"`
## Grep only tracked files with extension
`git grep -e "pattern" -- **/*.cpp`
## Grep in staging are
`git grep --cached "pattern"`
## Find cursed trailing spaces !
`git grep ' $' -- \*.h \*.cpp \*.qml`
## Grep all tracked files that have ";"in the end of the line and return that does not have a space after the n in all cpp files !
`git grep -e ";$" --and -e "return" --and --not -e "n " -- "**/*.cpp" # ULTIMATE POWER`
## Search for something in all commits
`git grep "something" $(git rev-list --all)`

# ls-files
## Show all tracked files
`git ls-files`

# Fetch
## Git fetch all remotes
`git fetch --all`

# diffs/patchs/archives
## Show diff from HEAD and stash
`git stash show -p stash@{0}`
`git diff stash@{0} HEAD`
## Shor diff of line between hashs
`git diff HEAD master -- myfile.md`
## Create a patch from the n commits from the hash
`git format-patch -n hash --stdout > magic_feature.patch`
## Create .tar.gz with all tracked files
`git archive --format=tar HEAD | gzip > example.tar.gz`

# Add
## Add in interactive
`git add -p`
## Add and create a commit
`git commit -p`
## Add path from root folder (: = project root folder)
`git add :/path`

# Rebase
## Split an old commit in two
1. Start an interactive rebase: `git rebase -i HEAD~4`
2. Change one commit from 'pick' to 'edit' or 'e'. Close and save the file.
3. Clean the commit to create a new one: `git reset HEAD~`
4. Add things to the first commit: `git add -p` and `git commit -sm "nice"`
5. Add things to the second commit: `git add -p` and `git commit -sm "feature"`
6. Finish the rebase: `git rebase --continue`
## Now you have two commits !
## Rebase in interactive mode the last 3 commits
`git rebase -i HEAD~3`

# Commit
## Commit a message with a sign
`git commit -sm "some awesome new crazy patch"`
## Merge the last uncommited added files
`git commit --amend`

# Checkout
## Checkout and create branch
`git checkout -b branch`
## Checkout some parts of the code in interactive format
`git checkout -p`
## Go back last branch
`git checkout -`
## Checkout file to vertion in hash
`git checkout c5f567 -- file`

# Branchs information
## List last branchs
`git branch --sort=committerdate`
## Or the long version
`git for-each-ref --sort='-authordate:iso8601' --format=' %(authordate:relative)%09%(refname:short)' refs/heads`

# Fetch
## Get a pull request
`git fetch origin pull/PR_NUMBER/head:LOCAL_BRANCH_NAME`

# Stash
## Stash in interactive
`git stash -p`
## Stash untracked files
`git stash save -u`
`git stash --include-untracked`

# Log
## Old style tig
`git log --graph`
## Show only the last 3 logs
`git log -3`
## Show only commits that have pattern
`git log --grep "patern"`
## Show all commits that change a file
`git log -- file`
## Show all commits with diffs !
`git log -p -- file`
## Show commits between two points (changelog)
`git log --oneline HEAD ^OLD_HASH`
## Show commits between two points (changelog) with format
`git log --oneline HEAD ^OLD_HASH --format=%s`
## Show history of lines in file
`git log -L start_line,+lines:path_file`

# diff-tree
## Show list of files that was modified in this branch compared to master
`git diff-tree --no-commit-id --name-only -r HEAD master`
## It's possible to interact with the files with for
`for x in $(git diff-tree --no-commit-id --name-only -r HEAD origin/master);do sed -i -e 's/-2016/-2018/g' $x; done;`

# Remote
## Show remotes information
`git remote -v`
## Delete remote
`git remote delete remote_name`

# Bisect
## Run git bisect with script
`git bisect run ./test-error.sh`
## Run git bisect with command
`git bisect run make`
## Run git bisect with inline commands
`git bisect run sh -c "rm -rf build && mkdir build && cd build && cmake .. && make -j5"`
> Note: Git bisect have a special exit code of 125, where should be used when no test can be done.

# Checkout
## Track remote branch
`git checkout remote/branch --track`

# Push
## Push all tags with all tracked branchs !
`git push remote --mirror`

# Mirror
## Create a mirror from a repository
```
git clone --bare repository
cd repository
git remote add mirror link
git push mirror --mirror
```

# Optimization and organization
## Cleanup unnecessary files and optimize the local repository
`git gc --prune=now --aggressive`
## Rename branch
`git branch -m old-name new-name`
## Rename current branch
`git branch -m new-name`
## Push with different name
`git push REMOTE LOCAL_BRANCH:REMOTE_BRANCH`
## Delete remote branch
`git push REMOTE :REMOTE_BRANCH`

# HG
## Create a hg mirror
`git clone hg::https://username@mail.com:pass@bitbucket.org/user/repo`

# Cherry-pick
## Cherry-pick range of commits
`git cherry-pick branch~4..branch`

# Push
## Push a tag or a branch with same name
`git push remote refs/heads/branch_name`
`git push remote refs/tags/tag_name`

# Config
## Save login/pass for some time in this session
`git config --global credential.helper "cache --timeout 7200"`

# Filter-branch
## Replace 'batata' with 'potato' in .py files from HEAD to HEAD~5
`git filter-branch -f --tree-filter "find . -name '*.py' -exec sed -i -e 's/batata/potato/g' {} \;" HEAD~5..HEAD`
## Replace 'batata' with 'potato in commit messages from HEAD to HEAD~5
`git filter-branch -f --msg-filter 'sed "s/batata/potato/g"' HEAD~5..HEAD`

# Util bash examples
## Add each individual modified file and commit it with name: message
`for file in $(git ls-files --modified); do git add $file; git commit -sm "${$(basename $file)%.*}: message"; done`
## Update banch1 and branch2 over origin/master and update my_remote
`for branch in branch1 branch2; do git rebase origin/master $branch; git push my_remote $branch --force; done`
## Delete all tags with a prefix name in remote or local
```bash
# Check login/pass timeout command
# Remove <prefix> tags
for tag in $(git tag | grep <prefix>); do git push <remote> :$tag; done
```


# .gitignore
`*.[oa] # Will ignore *.o and *.a`
`!exception # Ignore files except this one`
