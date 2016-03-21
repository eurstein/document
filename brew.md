title: brew

[TOC]

## [homebrew not working on OSX](http://stackoverflow.com/questions/24652996/homebrew-not-working-on-osx)
First, open terminal and `cd /usr/local/`, and `git status` to see if Homebrew is clean.

if dirty, `git reset --hard && git clean -df`

then `brew doctor`, `brew update`

If still broken, try this in your terminal:
```
$ sudo rm /System/Library/Frameworks/Ruby.framework/Versions/Current
$ sudo ln -s /System/Library/Frameworks/Ruby.framework/Versions/1.8 /System/Library/Frameworks/Ruby.framework/Versions/Current
```
This will force Homebrew to use ruby 1.8 from system
