---
title: "Missing Semester Lecture5: Command-line Environment"
date: 2024-04-22 22:48:25
tags: tmux
categories:
- [missing semester]
---
### Lecture 5: Command-line Environment

link: https://missing.csail.mit.edu/2020/command-line/

```bash
sleep 20
```

> sleep for 20 seconds

`<C-C>` : interrupt the commond

> it sends SIGINT

```bash
man signal
```

> read some signals

`<C-\>`: SIGQUIT

```bash
kill -TERM <PID>
```



While `SIGINT` and `SIGQUIT` are both usually associated with terminal related requests, a more generic signal for asking a process to exit gracefully is the `SIGTERM` signal. To send this signal we can use the [`kill`](https://www.man7.org/linux/man-pages/man1/kill.1.html) command, with the syntax `kill -TERM <PID>`.

`<C-Z>`:SIGTSTP, short for Terminal Stop

`bg`

`fg`

`jobs`

`kill`

`nohup`: To background an already running program you can do `Ctrl-Z` followed by `bg`. Note that backgrounded processes are still children processes of your terminal and will die if you close the terminal (this will send yet another signal, `SIGHUP`). To prevent that from happening you can run the program with [`nohup`](https://www.man7.org/linux/man-pages/man1/nohup.1.html) (a wrapper to ignore `SIGHUP`), or use `disown` if the process has already been started. Alternatively, you can use a terminal multiplexer as we will see in the next section.



```bash
$ sleep 1000
^Z
[1]  + 18653 suspended  sleep 1000

$ nohup sleep 2000 &
[2] 18745
appending output to nohup.out

$ jobs
[1]  + suspended  sleep 1000
[2]  - running    nohup sleep 2000

$ bg %1
[1]  - 18653 continued  sleep 1000

$ jobs
[1]  - running    sleep 1000
[2]  + running    nohup sleep 2000

$ kill -STOP %1
[1]  + 18653 suspended (signal)  sleep 1000

$ jobs
[1]  + suspended (signal)  sleep 1000
[2]  - running    nohup sleep 2000

$ kill -SIGHUP %1
[1]  + 18653 hangup     sleep 1000

$ jobs
[2]  + running    nohup sleep 2000

$ kill -SIGHUP %2

$ jobs
[2]  + running    nohup sleep 2000

$ kill %2
[2]  + 18745 terminated  nohup sleep 2000

$ jobs
```

### tmux(Terminal Multiplexers)

- Sessions
  - Windows(Tabs)
    - Panes

### split panes using | and -

bind | split-window -h
bind - split-window -v
unbind '"'
unbind %

### switch panes using Alt-hjkl without prefix

bind -n M-h select-pane -L
bind -n M-l select-pane -R
bind -n M-k select-pane -U
bind -n M-j select-pane -D

### default tmux setting

C-a C-o     Rotate through the panes                           [38/38]
C-a C-z     Suspend the current client
C-a Space   Select next layout
C-a !       Break pane to a new window
C-a #       List all paste buffers
C-a $       Rename current session
C-a &       Kill current window
C-a '       Prompt for window index to select
C-a (       Switch to previous client
C-a )       Switch to next client
C-a ,       Rename current window
C-a .       Move the current window
C-a /       Describe key binding
C-a 0       Select window 0
C-a 1       Select window 1
C-a 2       Select window 2
C-a 3       Select window 3
C-a 4       Select window 4
C-a 5       Select window 5
C-a 6       Select window 6
C-a 7       Select window 7
C-a 8       Select window 8
C-a 9       Select window 9
C-a :       Prompt for a command
C-a ;       Move to the previously active pane
C-a =       Choose a paste buffer from a list
C-a ?       List key bindings
C-a C       Customize options
C-a D       Choose a client from a list
C-a E       Spread panes out evenly
C-a L       Switch to the last client
C-a M       Clear the marked pane
C-a [       Enter copy mode
C-a ]       Paste the most recent paste buffer
C-a c       Create a new window
C-a d       Detach the current client
C-a f       Search for a pane
C-a i       Display window information
C-a l       Select the previously current window
C-a m       Toggle the marked pane




