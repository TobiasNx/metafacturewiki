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

It's good habit to use semantic versioning in release numbers "A.B.C", i.e. increase "A" when it's a major release breaking backward compatibility; increase "B" when it got new features; increase "C" indicating bug-fixes.

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
    ./gradlew assemble
    ```
 1. Finally, push the new tag to GitHub to trigger the actual release build:
    ```
    git push --follow-tags metafacture-core-VERSION
    ```
  
## Github
A release build is triggered on Travis CI by pushing the annotated git tag to the [metafacture/metafacture-core](https://github.com/metafacture/metafacture-core) repository. Ideally, this tag is also GPG signed. This signature appears on the [github-release](https://github.com/metafacture/metafacture-core/releases).
At the github-release page appears also the releaser, identified by setting the `GITHUB_RELEASE_TOKEN` in the travis-setting.

## Sonatype
Our build configuration for Travis CI recognises annotated tags and automatically performs a release build and publishes it to sonatype, where it is published to Maven Central. See also the varies travis variables used for this workflow: the `KEY*` for getting the key and signing the releases which are build by travis via maven and then pushed to [nexus at sonatype](https://oss.sonatype.org/) by using the `ORG_GRADLE_PROJECT_*` and `SONARCLOUD_TOKEN` variables.


