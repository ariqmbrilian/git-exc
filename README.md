# Git beginner
basic configuration
```
git config --global user.name "[username]"
git config --global user.email "[email@example.org]"
```
set default editor git with vscode
```
git config --global core.editor "code --wait"
```
set default difftool
```
git config --global diff.tool "default-difftool"
git config --global difftool.default-difftool.cmd "code --wait --diff \$LOCAL \$REMOTE"
```
show global config
```
git config --list --show-origin
```
initialize working directory
```
git init
```
show status
```
git status
```
add file to staged
```
git add [file_name]
git add .
```
commit staged file to permanently repository
```
git commit -m "[message_name]"
```
cancel adding file
```
git clean -f
```
restore changes
```
git restore [file_name]
```