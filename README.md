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
initialize repository
```
git init
```