# Merging pull requests

Pull requests should always be rebased to the latest master and merge as a none fast-forward merge.
For example, to merge a pull request fix-xy from http://github.com/cboehme/metafacture-core the following git commands should be used:
```
# Bring master up-to-date:
git checkout master
git fetch
git rebase
# Fetch pull request:
git fetch http://github.com/cboehme/metafacture-core.git +fix-xy:cboehme-fix-xy
git checkout cboehme-fix-xy
# Rebase the pull request:
git rebase master
# Run test cases, check commit, add fixup commits ...
# Merge with master
git checkout master
git merge --no-ff cboehme-fix-xy
```
The commit message of the merge command should follow this format:
```
Resolve #PULLREQUEST-ID: merge branch 'cboehme-fix-xy'

Pull request with commits:
 - First line of first commit's message
 - First line of second commit's message
 - ....
```