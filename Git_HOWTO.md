Git HOWTO

`diff --git` in the output of `git diff` is an "imaginary diff option":
```sh
$ git diff HEAD~1..HEAD | head
diff --git Documentation/git.txt Documentation/git.txt
index bd659c4..7913fc2 100644
--- Documentation/git.txt
+++ Documentation/git.txt
@@ -43,6 +43,11 @@ unreleased) version of Git, that is available from the 'master'
 branch of the `git.git` repository.
 Documentation for older releases are available here:

+* link:v2.10.0/git.html[documentation for release 2.10]
+
$
```

*git ssh*

```shell
git remote add origin ssh://user@server:/GitRepos/myproject.git
git push origin master
```
