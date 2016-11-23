# Git: Ignoring changes in tracked files

There are 3 options, you probably want #3

## 1. git rm --cached

This will keep the local file for you, but delete it locally for anyone else who pulls

```bash
git rm --cached <file-name> or git rm -r --cached <folder-name>
```

## 2. git update-index --asume-unchanged

This is designed more to optimize when a large number of files are added, e.g. SDKs that probably won't ever change. So you just tell git to stop checking that huge folder every time for changes, since it won't have any.

```bash
git update-index --assume-unchanged <file-name>
```

## 3. git update-index --skip-worktree

Tell git to assume each "git directory" has its own independent version of the file. For instance, you don't want to overwrite (or delete) production/staging config files.

```bash
git update-index --skip-worktree <file-name>
```

It's important to know that git update-index will not propagate with git,
and each user will have to run it independently.

Taken from: http://stackoverflow.com/a/40272289/6598709
