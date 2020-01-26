# **Git Essentials**

## First Steps

1. Go to your directory and execute this command:
```
git init
```
2. It will make a hidden directory `.git`
3. From this moment everything controlled by git

## Useful Commands

- Check status of files
```
git status
```
- Help
```
git help COMMAND
```
- Changed files (untracked) -> Stage (Index) (tracked) -> Commit (HEAD) (Saved in archive of git => .git)
```
GO TO STAGE: git add FILE_NAME | git add "*.py" | git add -A | git add --all
GO TO COMMIT: git commit -m "MESSAGE OF THIS COMMIT"
```
- Add changes to the last commit without multiple commits
```
git commit --amend
```
- HEAD: Where we are working on | our last commit
- Skip staging and commit, and add changes from all tracked files. This doesn't add new untracked files.
```
git commit -a -m "MESSAGE OF THIS COMMIT"
```
- Don't need files or folders to be tracked by Git -> (type filenames `hello.exe` or file extensions `*.zip` line by line)
```bash
$ touch .gitignore
```
- Log of every changes and commits
```
git log
```
- Show the differences in last commit
```
git diff HEAD
```
- Show the differences in stage
```
git diff --staged
```
- Undo a staged file to "Changed file"
```
git reset FILE_NAME
```
- Reset into staging and move to commit before HEAD
```
git reset --soft HEAD^
```
- Undo last commit and all changes
```
git reset --hard HEAD^
```
- Undo last 2 commits and all changes
```
git reset --hard HEAD^^
```
- Send a file to its last commit and forget new changes
```
git checkout -- FILE_NAME
```
- Remove a file from git and file system
```
git rm FILE_NAME
```
- Clone a project (ADDRESS is also called ORIGIN)
```
git clone ADDRESS
```
- Push my project/changes to GitHub (origin is ahead of our master)
```
git push origin master
git push -u origin master -> after this command you can only use -> git push
```
- Update my clone from origin
```
git pull origin master
```
- Show details of changes in specific commit
```
git show COMMIT_HASH
```

## Branch

- Show branches
```
git branch
```
- Create a branch
```
git branch BRANCH_NAME
```
- Create a branch and go to it
```
git branch -b BRANCH_NAME
```
- Enter a branch
```
git checkout BRANCH_NAME
```
- Merge a branch with master
```
git merge BRANCH_NAME
```
- Delete a branch
```
git branch -d BRANCH_NAME
```

## Remote and Conflict

- Show remotes
```
git remote
```
- Add a remote for GitHub or your company internal git address (Address can be SSH, HTTPS or HTTP)
```
git remote add origin ADDRESS
```
- Verbose or be talkative
```
git remote -v
```

## Tag

- List of tags
```
git tag
```
- List of tags with search (VERSION_NAME can be wildcard)
```
git tag -l VERSION_NAME
```
- Show log for a specific tag
```
git show VERSION_NAME
```
- Assign a tag (-a = annotate, VERSION_NAME can be _v1.0_)
```
git tag -a VERSION_NAME -m "MESSAGE"
```
- Assign a tag for older commits
```
git tag -a VERSION_NAME COMMIT_HASH -m "MESSAGE"
```
- You can't push tags by default. You have to specifically tell git to do it.
```
git push origin VERSION_NAME
git push origin --tags
```

## Sign Tags and Commits

> PGP: Pretty Good Privacy

> GPG: GNU Privacy Guard

- List of keys
```bash
$ gpg --list-keys
```
- Create a key
```bash
$ gpg --gen-key
```
- Show list of secret key
```bash
$ gpg --list-secret-keys --keyid-format LONG
```
- Commands for keys
```
git config --global user.name
git config --global user.signingkey
git config --global user.signingkey KEY
```
- Sign a tag
```
git tag -s VERSION_NAME -m "MESSAGE"
git show VERSION_NAME
```
- Verify a tag signature
```
git tag -v VERSION_NAME
```
- Sign a commit
```
git commit -S -m "MESSAGE"
```

## Debugging

- Blame/Check someone
```
git blame FILE_NAME -> Analyze all of lines in FILE_NAME
git blame FILE_NAME -LLINE_NUMBER
    git blame README.md -L8    -> Analyze line 8
    git blame README.md -L8,10 -> Analyze from line 8 to line 10
```
- BISECT (BInary SEarch CommiT)
```
git bisect start -> start from toplevel of the working tree
git bisect bad -> I'm telling you that something here is working bad
git bisect good COMMIT_HASH -> I'm telling you that something here was working good
```
