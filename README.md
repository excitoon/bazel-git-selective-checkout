Tool for selectively checking out Git Bazel repositories skipping extra files.

### How to use Git sparse checkout

#### With bare hands

```
mkdir <base-name>
cd <base-name>
git init
git config core.sparseCheckout true
git remote add -f origin <repository-url>
echo "WORKSPACE" > .git/info/sparse-checkout
echo "some" >> .git/info/sparse-checkout
echo "third_party/some/dependency" >> .git/info/sparse-checkout
echo "third_party/some/other/dependency" >> .git/info/sparse-checkout
git checkout
bazel build //some:target
```

#### With `bgsc`

```
bgsc clone <repository-url>
cd <base-name>
bgsc checkout //some:target
bazel build //some:target
```

### Example

```
C:\Users\vladi\Documents>bgsc clone file://c:/users/vladi/documents/src/test-project
Initialized empty Git repository in C:/Users/vladi/Documents/test-project/.git/
Updating origin
remote: Counting objects: 13, done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 13 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (13/13), done.
From file://c:/users/vladi/documents/src/test-project
 * [new branch]      master     -> origin/master
Already on 'master'
Branch master set up to track remote branch master from origin.

C:\Users\vladi\Documents>cd test-project

C:\Users\vladi\Documents\test-project>bgsc checkout //some:target
Adding /some/
Already on 'master'
Your branch is up-to-date with 'origin/master'.
bazel query deps(//some:target)
Adding /third_party/some/dependency/
Adding /third_party/some/other/dependency/
Already on 'master'
Your branch is up-to-date with 'origin/master'.
bazel query deps(//some:target)
Successfully checked out. Total steps: 2
```
