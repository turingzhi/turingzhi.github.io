---
title: "Missing Semester Lecture2: Shell Tools and Scripting"
date: 2024-04-15 19:23:21
tags: shell
categories:
- missing semester
---
### Lecture 2: Shell Tools and Scripting

link:[https://missing.csail.mit.edu/2020/shell-tools/](https://missing.csail.mit.edu/2020/shell-tools/)

```bash
echo "Hello"
> Hello
echo "World"
> World
foo=bar(good)
foo = bar(not work)
echo "Value is $foo"
> Value is bar
echo 'Value is $foo'
> Value is $foo
```

- `$0` - Name of the script
- `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
- `$@` - All the arguments
- `$#` - Number of arguments
- `$?` - Return code of the previous command
- `$$` - Process identification number (PID) for the current script
- `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
- `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.` or `Alt+.`
- `$!` contains the process ID of the most recently executed background pipeline

```bash
echo "We are in $(pwd)"
cat <(ls) <(ls ..)
ls *.sh
> show all files which are end with sh
ls project?
> ? means a single character
touch foo{1,2,3}
> and press tab
> it will be foo1, foo2, foo3
touch {foo,bar}/{a..j}
diff <(foo) <(bar)
```

`tldr` is a good tool to read guide of commands 

```bash
find
fd
rg
fzf
tree
broot
nnn
```



```bash
find . -maxdepth 1 -type f -name "*.html" -print0 | xargs -0 zip html_files.zip
find . -type f -exec stat -f "%m %N" {} + | sort -nr | awk '{print $2}'
```

`bash shortcuts`:https://skorks.com/2009/09/bash-shortcuts-for-maximum-productivity/

### Command Editing Shortcuts

- **Ctrl + a** – go to the start of the command line
- **Ctrl + e** – go to the end of the command line
- **Ctrl + k** – delete from cursor to the end of the command line
- **Ctrl + u** – delete from cursor to the start of the command line
- **Ctrl + w** – delete from cursor to start of word (i.e. delete backwards one word)
- **Ctrl + y** – paste word or text that was cut using one of the deletion shortcuts (such as the one above) after the cursor
- **Ctrl + xx** – move between start of command line and current cursor position (and back again)
- **Alt + b** – move backward one word (or go to start of word the cursor is currently on)
- **Alt + f** – move forward one word (or go to end of word the cursor is currently on)
- **Alt + d** – delete to end of word starting at cursor (whole word if cursor is at the beginning of word)
- **Alt + c** – capitalize to end of word starting at cursor (whole word if cursor is at the beginning of word)
- **Alt + u** – make uppercase from cursor to end of word
- **Alt + l** – make lowercase from cursor to end of word
- **Alt + t** – swap current word with previous
- **Ctrl + f** – move forward one character
- **Ctrl + b** – move backward one character
- **Ctrl + d** – delete character under the cursor
- **Ctrl + h** – delete character before the cursor
- **Ctrl + t** – swap character under cursor with the previous one

### Command Recall Shortcuts

- **Ctrl + r** – search the history backwards
- **Ctrl + g** – escape from history searching mode
- **Ctrl + p** – previous command in history (i.e. walk back through the command history)
- **Ctrl + n** – next command in history (i.e. walk forward through the command history)
- **Alt + .** – use the last word of the previous command

### Command Control Shortcuts

- **Ctrl + l** – clear the screen
- **Ctrl + s** – stops the output to the screen (for long running verbose command)
- **Ctrl + q** – allow output to the screen (if previously stopped using command above)
- **Ctrl + c** – terminate the command
- **Ctrl + z** – suspend/stop the command

### Bash Bang (!) Commands

*Bash* also has some handy features that use the ! (bang) to allow you to do some funky stuff with *bash* commands.

- **!!** – run last command
- **!blah** – run the most recent command that starts with ‘blah’ (e.g. !ls)
- **!blah:p** – print out the command that **!blah** would run (also adds it as the latest command in the command history)
- **!$** – the last word of the previous command (same as **Alt + .**)
- **!$:p** – print out the word that **!$** would substitute
- **!*** – the previous command except for the last word (e.g. if you type ‘_find some*file.txt /*’, then **!*** would give you ‘_find some*file.txt*’)
- **!\*:p** – print out what **!*** would substitute
- **$!** contains the process ID of the most recently executed background pipeline







