---
title: "Missing Semester Lecture3: Editor(vim)"
date: 2024-04-19 02:44:29
tags: vim
categories:
- [missing semester]

---
### Lecture 3: Editors(vim)

link: https://missing.csail.mit.edu/2020/editors/

normal->insert: i

insert->normal: <esc>

normal->replace: R

nomral->visual: v

normal->visual-line: <s-v>

noraml->visual-block: <c-v>

normal->command-line: **:**

```bash
:help :w
```

> show the usage of ':w'

shift + 6(^) : move the cursor to the beginning of the line where is not empty.

L: move the cursor to the lowest of the screen

M: middle

H: highest

fo: jump the next first o

Fo: jump to the previous o

to: jump to before the next first o

To: jump to after the previous first o

in nomral mode: c = d + i (c is short for change)

cc = dd + i

<C-R>: redo

undo is undo all the insert change or last change in normal mode

v: block

V: rectangle

ci[ : delete the contents that are inside the brackets and go to the insert mode.

ca[: delete the contents that are around the brackets and go to the insert mode

%: jump to the other parentheses

a: a is short for append

A: append at the end of this line

~: change the uppercase and lowercase

U: return the line to its original state.

<C-G>: show your location in the file and the file status.

number + G: return to the line with the number

To search for the same phrase again, simply type  n .
To search for the same phrase in the opposite direction, type  N .
To search for a phrase in the backward direction, use  ?  instead of  / .
To go back to where you came from press  CTRL-O  (Keep Ctrl down while pressing the letter o).  Repeat to go back further.  CTRL-I goes forward.

s: x+i

:s/thee/the: Note that this command only changes the first occurrence of "thee" in the line.

:s/thee/the/g .  Adding the  g  flag means to substitute globally in the line, change all occurrences of "thee" in the line.

To substitute new for the first old in a line type    :s/old/new
To substitute new for all 'old's on a line type       :s/old/new/g
To substitute phrases between two line #'s type       :#,#s/old/new/g
To substitute all occurrences in the file type        :%s/old/new/g
 To ask for confirmation each time add 'c'             :%s/old/new/gc

Type  :!  followed by an external command to execute that command.

Now type:   :w TEST   (where TEST is the filename you chose.) This saves the whole file (the Vim Tutor) under the name TEST.

:!command  executes an external command.
Some useful examples are:
:!ls            -  shows a directory listing.
:!rm FILENAME   -  removes file FILENAME.
:w FILENAME  writes the current Vim file to disk with name FILENAME.
v  motion  :w FILENAME  saves the Visually selected lines in file FILENAME.
:r FILENAME  retrieves disk file FILENAME and puts it below the cursor position.
:r !dir  reads the output of the dir command and puts it below the cursor position.




