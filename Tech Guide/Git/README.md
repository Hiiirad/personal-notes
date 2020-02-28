# **Git Essentials**

## First Steps

1. Install git:
```bash
sudo apt install git-all
git --version
```
2. Go to your directory and execute this command:
```bash
git init
```
3. It will make a hidden directory `.git`
4. From this moment everything controlled by git

## Useful Commands

- Check status of files
```bash
git status
```
- Help
```bash
git help COMMAND
```
- Changed files (untracked) -> Stage (Index) (tracked) -> Commit (HEAD) (Saved in archive of git => .git)
```
GO TO STAGE: git add FILE_NAME | git add "*.py" | git add -A | git add --all
GO TO COMMIT: git commit -m "MESSAGE OF THIS COMMIT"
```
- Add changes to the last commit without multiple commits
```bash
git commit --amend
```
- HEAD: Where we are working on | our last commit
- Skip staging and commit, and add changes from all tracked files. This doesn't add new untracked files.
```bash
git commit -a -m "MESSAGE OF THIS COMMIT"
```
- Don't need files or folders to be tracked by Git -> (type filenames `hello.exe` or file extensions `*.zip` line by line)
```bash
$ touch .gitignore
```
- Log of every changes and commits
```bash
git log
```
- Show the differences in last commit
```bash
git diff HEAD
```
- Show the differences in stage
```bash
git diff --staged
```
- Undo a staged file to "Changed file"
```bash
git reset FILE_NAME
```
- Reset into staging and move to commit before HEAD
```bash
git reset --soft HEAD^
```
- Undo last commit and all changes
```bash
git reset --hard HEAD^
```
- Undo last 2 commits and all changes
```bash
git reset --hard HEAD^^
```
- Send a file to its last commit and forget new changes
```bash
git checkout -- FILE_NAME
```
- Remove a file from git and file system
```bash
git rm FILE_NAME
```
- Clone a project (ADDRESS is also called ORIGIN)
```bash
git clone ADDRESS
```
- Push my project/changes to GitHub (origin is ahead of our master)
```bash
git push origin master
git push -u origin master -> after this command you can only use -> git push
```
- Update my clone from origin
```bash
git pull origin master
```
- Show details of changes in specific commit
```bash
git show COMMIT_HASH
```

## Branch

- Show branches
```bash
git branch
```
- Create a branch
```bash
git branch BRANCH_NAME
```
- Create a branch and go to it
```bash
git branch -b BRANCH_NAME
```
- Enter a branch
```bash
git checkout BRANCH_NAME
```
- Merge a branch with master
```bash
git merge BRANCH_NAME
```
- Delete a branch
```bash
git branch -d BRANCH_NAME
```

## Remote and Conflict

- Show remotes
```bash
git remote
```
- Add a remote for GitHub or your company internal git address (Address can be SSH, HTTPS or HTTP)
```bash
git remote add origin ADDRESS
```
- Verbose or be talkative
```bash
git remote -v
```

## Tag

- List of tags
```bash
git tag
```
- List of tags with search (VERSION_NAME can be wildcard)
```bash
git tag -l VERSION_NAME
```
- Show log for a specific tag
```bash
git show VERSION_NAME
```
- Assign a tag (-a = annotate, VERSION_NAME can be _v1.0_)
```bash
git tag -a VERSION_NAME -m "MESSAGE"
```
- Assign a tag for older commits
```bash
git tag -a VERSION_NAME COMMIT_HASH -m "MESSAGE"
```
- You can't push tags by default. You have to specifically tell git to do it.
```bash
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
```bash
git config --global user.name
git config --global user.signingkey
git config --global user.signingkey KEY
```
- Sign a tag
```bash
git tag -s VERSION_NAME -m "MESSAGE"
git show VERSION_NAME
```
- Verify a tag signature
```bash
git tag -v VERSION_NAME
```
- Sign a commit
```bash
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
