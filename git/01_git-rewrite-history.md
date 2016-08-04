# Rewriting git history.
I needed to rewrite the git history of a repository to edit out a text file containing sensitive information.

First I need to revert to the commit that introduced the sensitive information but there has been 2 commits after the offending commit.
Anyway it turns out that the last 2 commits are benign, they can easily be reapplied after rewriting the git history so for now, 
I will discard the changes the last 2 commits introduced by doing a hard reset.


# 1. Move to the commit which contains text to be excluded from the commit history (but this will discard the last 2 commits)
```
cd git-repo-path/
git reset --hard b9b482a66ef292d88f33e3308f629f52e16b6b0e
```

# 2. Edit the offending file(s) in the last commit to remove sensitive info
```
cd git-repo-path/
vim path/to/file/FILE-CONTAINING-PRIVATE-TEXT.md
git commit -a --amend
```


# 3. Unfortunately, there is another file which is deep in the commit history with sensitive info.
We will use `bfg` to remove its commit history; it will be added back later after the sensitive info has been edited out.

## 3.1. Delete the file from git by moving it out of the git folder.
```
cd git-repo-path/
mv OLDER-FILE-CONTAINING-PRIVATE-TEXT.md ../
git add .
git commit -m "Removed file from repo."
```

## 3.2. Use `bfg` to rewrite the git history 
```
cd git-repo-path/
java -jar ../bfg-1.12.12.jar --delete-files OLDER-FILE-CONTAINING-PRIVATE-TEXT.md  .git
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

# 3.3. Add back the file
```
mv ../OLDER-FILE-CONTAINING-PRIVATE-TEXT.md .
```

# 3.4. Make all necessary modifications to the file
```
vim OLDER-FILE-CONTAINING-PRIVATE-TEXT.md
git commit -am "Added cleaned up file back to repo."
```






