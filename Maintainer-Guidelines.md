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
Merge pull-request #PULLREQUEST-ID from cboehme/fix-xy
```

# Making a release

A release build is triggered on Travis CI by pushing an annotated git tag to the [metafacture/metafacture-core](https://github.com/metafacture/metafacture-core) repository. Ideally, this tag is also GPG signed. Our build configuration for Travis CI recognises annotated tags and automatically performs a release build and publishes it to GitHub and Maven Central.
The following commands trigger a release build.

 1. Create a signed tag:
    ```
    git tag -s metafacture-core-VERSION
    ```
 1. When prompted, add a sensible commit message. For instance, something like:
    ```
    Publish first release of the Metafacture X line
    ```
 1. Optionally, you can now test the build locally by invoking a gradle target:
    ```
    ./gradlew install
    ```
 1. Finally, push the new tag to GitHub to trigger the actual release build:
    ```
    git push origin metafacture-core-VERSION
    ```