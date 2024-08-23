---
title: "Missing Semester Lecture7: Debugging and Profiling"
date: 2024-07-11 06:13:55
tags: 
- debug
- profiling
categories:
- [missing semester]
---
#### Lecture 7: Debugging and Profiling

link:[https://missing.csail.mit.edu/2020/debugging-profiling/](https://missing.csail.mit.edu/2020/debugging-profiling/)

### Printf debugging and Logging

* A first approach to debug a program is to add print statements around where you have detected the problem, and keep iterating until you have extracted enough information to understand what is responsible for the issue.
* A second approach is to use logging in your program, instead of ad hoc print statements. Logging is better than regular print statements for several reasons:
  * You can log to files, sockets or even remote servers instead of standard output.
  * Logging supports severity levels (such as INFO, DEBUG, WARN, ERROR, &c), that allow you to filter the output accordingly.
  * For new issues, there’s a fair chance that your logs will contain enough information to detect what is going wrong.

```bash
$ python logger.py
# Raw output as with just prints
$ python logger.py log
# Log formatted output
$ python logger.py log ERROR
# Print only ERROR levels and above
$ python logger.py color
# Color formatted output
```

> The most important way of making logs more readable is to color code them. By now you probably have realized that your terminal uses colors to make things more readable. But how does it do it? Programs like `ls` or `grep` are using [ANSI escape codes](https://en.wikipedia.org/wiki/ANSI_escape_code), which are special sequences of characters to indicate your shell to change the color of the output. For example, executing `echo -e "\e[38;2;255;0;0mThis is red\e[0m"` prints the message `This is red`in red on your terminal, as long as it supports [true color](https://github.com/termstandard/colors#truecolor-support-in-output-devices). If your terminal doesn’t support this (e.g. macOS’s Terminal.app), you can use the more universally supported escape codes for 16 color choices, for example `echo -e "\e[31;1mThis is red\e[0m"`.

### Third party logs

> most programs write their own logs somewhere in your system. In UNIX systems, it is commonplace for programs to write their logs under `/var/log`. For instance, the [NGINX](https://www.nginx.com/) webserver places its logs under `/var/log/nginx`. 
>
> More recently, systems have started using a **system log**, which is increasingly where all of your log messages go. Most (but not all) Linux systems use `systemd`, a system daemon that controls many things in your system such as which services are enabled and running. `systemd` places the logs under `/var/log/journal` in a specialized format and you can use the [`journalctl`](https://www.man7.org/linux/man-pages/man1/journalctl.1.html)command to display the messages. 
>
> Similarly, on macOS there is still `/var/log/system.log` but an increasing number of tools use the system log, that can be displayed with [`log show`](https://www.manpagez.com/man/1/log/). 
>
> On most UNIX systems you can also use the [`dmesg`](https://www.man7.org/linux/man-pages/man1/dmesg.1.html) command to access the kernel log.

> For logging under the system logs you can use the [`logger`](https://www.man7.org/linux/man-pages/man1/logger.1.html) shell program. Here’s an example of using `logger` and how to check that the entry made it to the system logs. Moreover, most programming languages have bindings logging to the system log.

```bash
logger "Hello Logs"
# On macOS
log show --last 1m | grep Hello
# On Linux
journalctl --since "1m ago" | grep Hello
```

### Debugger

When printf debugging is not enough you should use a debugger. Debuggers are programs that let you interact with the execution of a program, allowing the following:

- Halt execution of the program when it reaches a certain line.
- Step through the program one instruction at a time.
- Inspect values of variables after the program crashed.
- Conditionally halt the execution when a given condition is met.
- And many more advanced features

Many programming languages come with some form of debugger. In Python this is the Python Debugger [`pdb`](https://docs.python.org/3/library/pdb.html).

Here is a brief description of some of the commands `pdb` supports:

- **l**(ist) - Displays 11 lines around the current line or continue the previous listing.
- **s**(tep) - Execute the current line, stop at the first possible occasion.
- **n**(ext) - Continue execution until the next line in the current function is reached or it returns.
- **b**(reak) - Set a breakpoint (depending on the argument provided).
- **p**(rint) - Evaluate the expression in the current context and print its value. There’s also **pp** to display using [`pprint`](https://docs.python.org/3/library/pprint.html) instead.
- **r**(eturn) - Continue execution until the current function returns.
- **q**(uit) - Quit the debugger

Note that since Python is an interpreted language we can use the `pdb` shell to execute commands and to execute instructions. [`ipdb`](https://pypi.org/project/ipdb/) is an improved `pdb` that uses the [`IPython`](https://ipython.org/) REPL enabling tab completion, syntax highlighting, better tracebacks, and better introspection while retaining the same interface as the `pdb` module.

For more low level programming you will probably want to look into [`gdb`](https://www.gnu.org/software/gdb/) (and its quality of life modification [`pwndbg`](https://github.com/pwndbg/pwndbg)) and [`lldb`](https://lldb.llvm.org/). They are optimized for C-like language debugging but will let you probe pretty much any process and get its current machine state: registers, stack, program counter, &c.

### Specialized Tools

Even if what you are trying to debug is a black box binary there are tools that can help you with that. Whenever programs need to perform actions that only the kernel can, they use [System Calls](https://en.wikipedia.org/wiki/System_call). There are commands that let you trace the syscalls your program makes. In Linux there’s [`strace`](https://www.man7.org/linux/man-pages/man1/strace.1.html) and macOS and BSD have [`dtrace`](http://dtrace.org/blogs/about/). `dtrace` can be tricky to use because it uses its own `D` language, but there is a wrapper called [`dtruss`](https://www.manpagez.com/man/1/dtruss/) that provides an interface more similar to `strace` (more details [here](https://8thlight.com/blog/colin-jones/2015/11/06/dtrace-even-better-than-strace-for-osx.html)).

Below are some examples of using `strace` or `dtruss` to show [`stat`](https://www.man7.org/linux/man-pages/man2/stat.2.html) syscall traces for an execution of `ls`. For a deeper dive into `strace`, [this article](https://blogs.oracle.com/linux/strace-the-sysadmins-microscope-v2) and [this zine](https://jvns.ca/strace-zine-unfolded.pdf) are good reads.

```bash
# On Linux
sudo strace -e lstat ls -l > /dev/null
# On macOS
sudo dtruss -t lstat64_extended ls -l > /dev/null
```

Under some circumstances, you may need to look at the network packets to figure out the issue in your program. Tools like [`tcpdump`](https://www.man7.org/linux/man-pages/man1/tcpdump.1.html) and [Wireshark](https://www.wireshark.org/) are network packet analyzers that let you read the contents of network packets and filter them based on different criteria.

For web development, the Chrome/Firefox developer tools are quite handy. They feature a large number of tools, including:

- Source code - Inspect the HTML/CSS/JS source code of any website.
- Live HTML, CSS, JS modification - Change the website content, styles and behavior to test (you can see for yourself that website screenshots are not valid proofs).
- Javascript shell - Execute commands in the JS REPL.
- Network - Analyze the requests timeline.
- Storage - Look into the Cookies and local application storage.

> For some issues you do not need to run any code. For example, just by carefully looking at a piece of code you could realize that your loop variable is shadowing an already existing variable or function name; or that a program reads a variable before defining it. Here is where [static analysis](https://en.wikipedia.org/wiki/Static_program_analysis) tools come into play. Static analysis programs take source code as input and analyze it using coding rules to reason about its correctness.
>
> In the following Python snippet there are several mistakes. First, our loop variable `foo` shadows the previous definition of the function `foo`. We also wrote `baz` instead of `bar` in the last line, so the program will crash after completing the `sleep` call (which will take one minute).

```bash
import time

def foo():
    return 42

for foo in range(5):
    print(foo)
bar = 1
bar *= 0.2
time.sleep(60)
print(baz)
```

Static analysis tools can identify these kinds of issues. When we run [`pyflakes`](https://pypi.org/project/pyflakes)on the code we get the errors related to both bugs. [`mypy`](http://mypy-lang.org/) is another tool that can detect type checking issues. Here, `mypy` will warn us that `bar` is initially an `int` and is then casted to a `float`. Again, note that all these issues were detected without having to run the code.

```bash
$ pyflakes foobar.py
foobar.py:6: redefinition of unused 'foo' from line 3
foobar.py:11: undefined name 'baz'

$ mypy foobar.py
foobar.py:6: error: Incompatible types in assignment (expression has type "int", variable has type "Callable[[], Any]")
foobar.py:9: error: Incompatible types in assignment (expression has type "float", variable has type "int")
foobar.py:11: error: Name 'baz' is not defined
Found 3 errors in 1 file (checked 1 source file)
```

In the shell tools lecture we covered [`shellcheck`](https://www.shellcheck.net/), which is a similar tool for shell scripts.

Most editors and IDEs support displaying the output of these tools within the editor itself, highlighting the locations of warnings and errors. This is often called **code linting** and it can also be used to display other types of issues such as stylistic violations or insecure constructs.

In vim, the plugins [`ale`](https://vimawesome.com/plugin/ale) or [`syntastic`](https://vimawesome.com/plugin/syntastic) will let you do that. For Python, [`pylint`](https://github.com/PyCQA/pylint)and [`pep8`](https://pypi.org/project/pep8/) are examples of stylistic linters and [`bandit`](https://pypi.org/project/bandit/) is a tool designed to find common security issues. For other languages people have compiled comprehensive lists of useful static analysis tools, such as [Awesome Static Analysis](https://github.com/mre/awesome-static-analysis) (you may want to take a look at the *Writing* section) and for linters there is [Awesome Linters](https://github.com/caramelomartins/awesome-linters).

A complementary tool to stylistic linting are code formatters such as [`black`](https://github.com/psf/black) for Python, `gofmt` for Go, `rustfmt` for Rust or [`prettier`](https://prettier.io/) for JavaScript, HTML and CSS. These tools autoformat your code so that it’s consistent with common stylistic patterns for the given programming language. Although you might be unwilling to give stylistic control about your code, standardizing code format will help other people read your code and will make you better at reading other people’s (stylistically standardized) code.

### Profiling

#### Time

Similarly to the debugging case, in many scenarios it can be enough to just print the time it took your code between two points. Here is an example in Python using the [`time`](https://docs.python.org/3/library/time.html) module.

```bash
import time, random
n = random.randint(1, 10) * 100

# Get current time
start = time.time()

# Do some work
print("Sleeping for {} ms".format(n))
time.sleep(n/1000)

# Compute time between start and now
print(time.time() - start)

# Output
# Sleeping for 500 ms
# 0.5713930130004883
```

However, wall clock time can be misleading since your computer might be running other processes at the same time or waiting for events to happen. It is common for tools to make a distinction between *Real*, *User* and *Sys* time. In general, *User* + *Sys* tells you how much time your process actually spent in the CPU (more detailed explanation [here](https://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1)).

- *Real* - Wall clock elapsed time from start to finish of the program, including the time taken by other processes and time taken while blocked (e.g. waiting for I/O or network)
- *User* - Amount of time spent in the CPU running user code
- *Sys* - Amount of time spent in the CPU running kernel code

For example, try running a command that performs an HTTP request and prefixing it with [`time`](https://www.man7.org/linux/man-pages/man1/time.1.html). Under a slow connection you might get an output like the one below. Here it took over 2 seconds for the request to complete but the process only took 15ms of CPU user time and 12ms of kernel CPU time.

```bash
$ time curl https://missing.csail.mit.edu &> /dev/null
real    0m2.561s
user    0m0.015s
sys     0m0.012s
```

#### Profiler

#### CPU

Most of the time when people refer to *profilers* they actually mean *CPU profilers*, which are the most common. There are two main types of CPU profilers: *tracing*and *sampling* profilers. Tracing profilers keep a record of every function call your program makes whereas sampling profilers probe your program periodically (commonly every millisecond) and record the program’s stack. They use these records to present aggregate statistics of what your program spent the most time doing. [Here](https://jvns.ca/blog/2017/12/17/how-do-ruby---python-profilers-work-) is a good intro article if you want more detail on this topic.

Most programming languages have some sort of command line profiler that you can use to analyze your code. They often integrate with full fledged IDEs but for this lecture we are going to focus on the command line tools themselves.

In Python we can use the `cProfile` module to profile time per function call. Here is a simple example that implements a rudimentary grep in Python:

```bash
#!/usr/bin/env python

import sys, re

def grep(pattern, file):
    with open(file, 'r') as f:
        print(file)
        for i, line in enumerate(f.readlines()):
            pattern = re.compile(pattern)
            match = pattern.search(line)
            if match is not None:
                print("{}: {}".format(i, line), end="")

if __name__ == '__main__':
    times = int(sys.argv[1])
    pattern = sys.argv[2]
    for i in range(times):
        for file in sys.argv[3:]:
            grep(pattern, file)
```

We can profile this code using the following command. Analyzing the output we can see that IO is taking most of the time and that compiling the regex takes a fair amount of time as well. Since the regex only needs to be compiled once, we can factor it out of the for.

```bash
$ python -m cProfile -s tottime grep.py 1000 '^(import|\s*def)[^,]*$' *.py

[omitted program output]

 ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     8000    0.266    0.000    0.292    0.000 {built-in method io.open}
     8000    0.153    0.000    0.894    0.000 grep.py:5(grep)
    17000    0.101    0.000    0.101    0.000 {built-in method builtins.print}
     8000    0.100    0.000    0.129    0.000 {method 'readlines' of '_io._IOBase' objects}
    93000    0.097    0.000    0.111    0.000 re.py:286(_compile)
    93000    0.069    0.000    0.069    0.000 {method 'search' of '_sre.SRE_Pattern' objects}
    93000    0.030    0.000    0.141    0.000 re.py:231(compile)
    17000    0.019    0.000    0.029    0.000 codecs.py:318(decode)
        1    0.017    0.017    0.911    0.911 grep.py:3(<module>)

[omitted lines]
```

A caveat of Python’s `cProfile` profiler (and many profilers for that matter) is that they display time per function call. That can become unintuitive really fast, especially if you are using third party libraries in your code since internal function calls are also accounted for. A more intuitive way of displaying profiling information is to include the time taken per line of code, which is what *line profilers* do.

For instance, the following piece of Python code performs a request to the class website and parses the response to get all URLs in the page:

```bash
#!/usr/bin/env python
import requests
from bs4 import BeautifulSoup

# This is a decorator that tells line_profiler
# that we want to analyze this function
@profile
def get_urls():
    response = requests.get('https://missing.csail.mit.edu')
    s = BeautifulSoup(response.content, 'lxml')
    urls = []
    for url in s.find_all('a'):
        urls.append(url['href'])

if __name__ == '__main__':
    get_urls()
```

If we used Python’s `cProfile` profiler we’d get over 2500 lines of output, and even with sorting it’d be hard to understand where the time is being spent. A quick run with [`line_profiler`](https://github.com/pyutils/line_profiler) shows the time taken per line:

```bash
$ kernprof -l -v a.py
Wrote profile results to urls.py.lprof
Timer unit: 1e-06 s

Total time: 0.636188 s
File: a.py
Function: get_urls at line 5

Line #  Hits         Time  Per Hit   % Time  Line Contents
==============================================================
 5                                           @profile
 6                                           def get_urls():
 7         1     613909.0 613909.0     96.5      response = requests.get('https://missing.csail.mit.edu')
 8         1      21559.0  21559.0      3.4      s = BeautifulSoup(response.content, 'lxml')
 9         1          2.0      2.0      0.0      urls = []
10        25        685.0     27.4      0.1      for url in s.find_all('a'):
11        24         33.0      1.4      0.0          urls.append(url['href'])
```

#### Memory

In languages like C or C++ memory leaks can cause your program to never release memory that it doesn’t need anymore. To help in the process of memory debugging you can use tools like [Valgrind](https://valgrind.org/) that will help you identify memory leaks.

In garbage collected languages like Python it is still useful to use a memory profiler because as long as you have pointers to objects in memory they won’t be garbage collected. Here’s an example program and its associated output when running it with [memory-profiler](https://pypi.org/project/memory-profiler/) (note the decorator like in `line-profiler`).

```bash
@profile
def my_func():
    a = [1] * (10 ** 6)
    b = [2] * (2 * 10 ** 7)
    del b
    return a

if __name__ == '__main__':
    my_func()
$ python -m memory_profiler example.py
Line #    Mem usage  Increment   Line Contents
==============================================
     3                           @profile
     4      5.97 MB    0.00 MB   def my_func():
     5     13.61 MB    7.64 MB       a = [1] * (10 ** 6)
     6    166.20 MB  152.59 MB       b = [2] * (2 * 10 ** 7)
     7     13.61 MB -152.59 MB       del b
     8     13.61 MB    0.00 MB       return a
```

### Visualization

Profiler output for real world programs will contain large amounts of information because of the inherent complexity of software projects. Humans are visual creatures and are quite terrible at reading large amounts of numbers and making sense of them. Thus there are many tools for displaying profiler’s output in an easier to parse way.

One common way to display CPU profiling information for sampling profilers is to use a [Flame Graph](http://www.brendangregg.com/flamegraphs.html), which will display a hierarchy of function calls across the Y axis and time taken proportional to the X axis. They are also interactive, letting you zoom into specific parts of the program and get their stack traces (try clicking in the image below).

![](http://www.brendangregg.com/FlameGraphs/cpu-bash-flamegraph.svg)

Call graphs or control flow graphs display the relationships between subroutines within a program by including functions as nodes and functions calls between them as directed edges. When coupled with profiling information such as the number of calls and time taken, call graphs can be quite useful for interpreting the flow of a program. In Python you can use the [`pycallgraph`](https://pycallgraph.readthedocs.io/)library to generate them.

![](https://upload.wikimedia.org/wikipedia/commons/2/2f/A_Call_Graph_generated_by_pycallgraph.png)

#### Resource Monitoring

Sometimes, the first step towards analyzing the performance of your program is to understand what its actual resource consumption is. Programs often run slowly when they are resource constrained, e.g. without enough memory or on a slow network connection. There are a myriad of command line tools for probing and displaying different system resources like CPU usage, memory usage, network, disk usage and so on.

- **General Monitoring** - Probably the most popular is [`htop`](https://htop.dev/), which is an improved version of [`top`](https://www.man7.org/linux/man-pages/man1/top.1.html). `htop` presents various statistics for the currently running processes on the system. `htop` has a myriad of options and keybinds, some useful ones are: `<F6>` to sort processes, `t` to show tree hierarchy and `h` to toggle threads. See also [`glances`](https://nicolargo.github.io/glances/) for similar implementation with a great UI. For getting aggregate measures across all processes, [`dstat`](http://dag.wiee.rs/home-made/dstat/) is another nifty tool that computes real-time resource metrics for lots of different subsystems like I/O, networking, CPU utilization, context switches, &c.
- **I/O operations** - [`iotop`](https://www.man7.org/linux/man-pages/man8/iotop.8.html) displays live I/O usage information and is handy to check if a process is doing heavy I/O disk operations
- **Disk Usage** - [`df`](https://www.man7.org/linux/man-pages/man1/df.1.html) displays metrics per partitions and [`du`](http://man7.org/linux/man-pages/man1/du.1.html) displays **d**isk **u**sage per file for the current directory. In these tools the `-h` flag tells the program to print with **h**uman readable format. A more interactive version of `du` is [`ncdu`](https://dev.yorhel.nl/ncdu) which lets you navigate folders and delete files and folders as you navigate.
- **Memory Usage** - [`free`](https://www.man7.org/linux/man-pages/man1/free.1.html) displays the total amount of free and used memory in the system. Memory is also displayed in tools like `htop`.
- **Open Files** - [`lsof`](https://www.man7.org/linux/man-pages/man8/lsof.8.html) lists file information about files opened by processes. It can be quite useful for checking which process has opened a specific file.
- **Network Connections and Config** - [`ss`](https://www.man7.org/linux/man-pages/man8/ss.8.html) lets you monitor incoming and outgoing network packets statistics as well as interface statistics. A common use case of `ss` is figuring out what process is using a given port in a machine. For displaying routing, network devices and interfaces you can use [`ip`](http://man7.org/linux/man-pages/man8/ip.8.html). Note that `netstat` and `ifconfig` have been deprecated in favor of the former tools respectively.
- **Network Usage** - [`nethogs`](https://github.com/raboof/nethogs) and [`iftop`](http://www.ex-parrot.com/pdw/iftop/) are good interactive CLI tools for monitoring network usage.

If you want to test these tools you can also artificially impose loads on the machine using the [`stress`](https://linux.die.net/man/1/stress) command.

#### Specialized tools

Sometimes, black box benchmarking is all you need to determine what software to use. Tools like [`hyperfine`](https://github.com/sharkdp/hyperfine) let you quickly benchmark command line programs. For instance, in the shell tools and scripting lecture we recommended `fd` over `find`. We can use `hyperfine` to compare them in tasks we run often. E.g. in the example below `fd` was 20x faster than `find` in my machine.

```bash
$ hyperfine --warmup 3 'fd -e jpg' 'find . -iname "*.jpg"'
Benchmark #1: fd -e jpg
  Time (mean ± σ):      51.4 ms ±   2.9 ms    [User: 121.0 ms, System: 160.5 ms]
  Range (min … max):    44.2 ms …  60.1 ms    56 runs

Benchmark #2: find . -iname "*.jpg"
  Time (mean ± σ):      1.126 s ±  0.101 s    [User: 141.1 ms, System: 956.1 ms]
  Range (min … max):    0.975 s …  1.287 s    10 runs

Summary
  'fd -e jpg' ran
   21.89 ± 2.33 times faster than 'find . -iname "*.jpg"'
```

As it was the case for debugging, browsers also come with a fantastic set of tools for profiling webpage loading, letting you figure out where time is being spent (loading, rendering, scripting, &c). More info for [Firefox](https://profiler.firefox.com/docs/) and [Chrome](https://developers.google.com/web/tools/chrome-devtools/rendering-tools).

**Shellcheck**

**PDB**

https://wil.yegelwel.com/pdb-pm/

https://github.com/spiside/pdb-tutorial

> `l(ist)` `ll(long list)`ll shows  you source code for the current function or frame. It's much better knowing which function you are in than an arbitrary 11 lines around your current position.
>
> ```bash
> (Pdb) l
>   4     def main():
>   5         print("Add the values of the dice")
>   6         print("It's really that easy")
>   7         print("What are you doing with your life.")
>   8         import pdb; pdb.set_trace()
>   9  ->     GameRunner.run()
>  10     
>  11     
>  12     if __name__ == "__main__":
>  13         main()
> [EOF]
> ```
>
> If we want to see the whole file, we can call the list function with the range 1 to 13 like so:
>
> ```bash
> (Pdb) l 1, 13
>   1     from dicegame.runner import GameRunner
>   2     
>   3     
>   4     def main():
>   5         print("Add the values of the dice")
>   6         print("It's really that easy")
>   7         print("What are you doing with your life.")
>   8         import pdb; pdb.set_trace()
>   9  ->     GameRunner.run()
>  10     
>  11     
>  12     if __name__ == "__main__":
>  13         main()
> ```

> `s(tep)`  Execute the current line, stop at the first possible occasion(either in a function that is called or in the current function).
>
> ```bash
> (Pdb) s
> --Call--
> > /Users/Development/pdb-tutorial/dicegame/runner.py(21)run()
> -> @classmethod
> ```
>
> ```bash
> (Pdb) l
>  16             total = 0
>  17             for die in self.dice:
>  18                 total += 1
>  19             return total
>  20     
>  21  ->     @classmethod
>  22         def run(cls):
>  23             # Probably counts wins or something.
>  24             # Great variable name, 10/10.
>  25             c = 0
>  26             while True:
> ```



> `n(ext)`  Continue execution until the next line in the current function is reached or it returns.
>
> ```bash
> (Pdb) n
> > /Users/Development/pdb-tutorial/dicegame/runner.py(27)run()
> -> while True:
> (Pdb) l
>  21         @classmethod
>  22         def run(cls):
>  23             # Probably counts wins or something.
>  24             # Great variable name, 10/10.
>  25             c = 0
>  26  ->         while True:
>  27                 runner = cls()
>  28     
>  29                 print("Round {}\n".format(runner.round))
>  30     
>  31                 for die in runner.dice:
> ```

> `b(reak)` 
>
> - Without argument, list all breaks.
> - With a line number argument, set a break at this line in the current file. With a function name, set a break at the first executable line of that function. If a second argument is present, it is a string specifying an expression which must evaluate to true before the breakpoint is honored.
>
> - The line number may be prefixed with a filename and a colon, to specify a breakpoint in another file (probably one that hasn't been loaded yet).  The file is searched for on sys.path; the .py suffix may be omitted.
>
> ```bash
> (Pdb) b 34
> Breakpoint 1 at /Users/Development/pdb-tutorial/dicegame/runner.py(34)run()
> (Pdb) c
> 
> [...] # prints some dice
> 
> > /Users/Development/pdb-tutorial/dicegame/runner.py(34)run()
> -> guess = input("Sigh. What is your guess?: ")
> ```
>
> ```bash
> (Pdb) b
> Num Type         Disp Enb   Where
> 1   breakpoint   keep yes   at /Users/Development/pdb-tutorial/dicegame/runner.py:34
>     breakpoint already hit 1 time
> ```

> `cl(ear)` To clear your break points, you can use the `cl(ear)` command followed by the breakpoint number which is found in the leftmost column of the above output. You can also clear all the breakpoints if you don't provide any arguments to the `clear` command.
>
> ```bash
> (Pdb) cl 1
> Deleted breakpoint 1 at /Users/Development/pdb-tutorial/dicegame/runner.py:34
> ```
>
> 

> `r(eturn)`  Continue execution until the current function returns.
>
> ```bash
> (Pdb) r
> --Return--
> > /Users/Development/pdb-tutorial/dicegame/runner.py(19)answer()->5
> -> return total
> (Pdb) l
>  14     
>  15         def answer(self):
>  16             total = 0
>  17             for die in self.dice:
>  18                 total += 1
>  19  ->         return total
>  20     
>  21         @classmethod
>  22         def run(cls):
>  23             # Probably counts wins or something.
>  24             # Great variable name, 10/10.
> (Pdb) 
> ```

> `continue` The `continue` command instructs the debugger to resume execution of the program until the next breakpoint is encountered, an exception is raised, or the program terminates.

> `!c` If there is a variable called. c, but this c maybe be misunderstood with c(ontinue). So we use `!c` to get the value of variable c
>
> ```bash
>  (Pdb) !c
>  0
> ```

> `commands`
>
> ```bash
> commands [bpnumber]
>         (com) ...
>         (com) end
>         (Pdb)
> 
> Specify a list of commands for breakpoint number bpnumber.
> ```
>
> `commands` will run python code or pdb commands that you specified whenever the stated breakpoint number is hit. Once you start the `commands` block, the prompt changes to `(com)`. The code/commands you write here function as if you had typed them at the `(Pdb)` prompt after getting to that breakpoint. Writing `end` will terminate the command and the prompt changes back to `(Pdb)` from `(com)`. I have found this of great use when I need to monitor certain variables inside of a loop as I don't need to print the values of the variables repeatedly. Let's see an example. Make sure to be at the root of the project in your terminal and type the following:
>
> ```bash
> python -m pdb main.py
> ```
>
> 
>
> Reach line `:8` and `s(tep)` into the `run()` method of the GameRunner class. Then, set up a breakpoint at `:17`.
>
> ```bash
> > /Users/Development/pdb-tutorial/main.py(8)main()
> -> GameRunner.run()  #This is line 8 in main.py
> (Pdb) s      
> --Call--
> > /Users/Development/pdb-tutorial/dicegame/runner.py(21)run()
> -> @classmethod 
> (Pdb) b 17
> Breakpoint 4 at /Users/Development/pdb-tutorial/dicegame/runner.py:17
> ```
>
> 
>
> This sets up the breakpoint, which has been given the number `4`, at the start of the loop inside the `answer()`method which is used to calculate the total values of the dice. Now, let's us use `commands` to print the value of the variable `total` when we hit this breakpoint.
>
> ```bash
> (Pdb) commands 4
> (com) print(f"The total value as of now is {total}")
> (com) end
> ```
>
> 
>
> We have now set up `commands` for breakpoint number 4 which will execute when we reach this breakpoint. Let us `c(ontinue)` and reach this breakpoint.
>
> ```bash
> (Pdb) c
> [...] # You will have to guess a number
> The total value as of now is 0
> > /Users/Development/pdb-tutorial/dicegame/runner.py(17)answer()
> -> for die in self.dice:
> (Pdb)
> ```
>
> 
>
> We see that out print statement executed upon reaching this breakpoint. Let's `c(ontinue)` again and see what happens.
>
> ```bash
> (Pdb) c
> [...] 
> The total value as of now is 1
> > /Users/Development/pdb-tutorial/dicegame/runner.py(17)answer()
> -> for die in self.dice:
> (Pdb)
> ```
>
> 
>
> The `commands` command executes upon reaching the breakpoint again. You can see how this might be useful especially during loops.

> ### `pdb.pm()` Post Mortem
>
> ```
> pdb.post_mortem(traceback=None)
>     Enter post-mortem debugging of the given traceback object. If no traceback is given, it uses the one of the exception that is currently being handled
>     (an exception must be being handled if the default is to be used).
> 
> pdb.pm()
>     Enter post-mortem debugging of the traceback found in sys.last_traceback. 
> ```
>
> 
>
> While both methods may look the same, `post_mortem() and pm()` differ by the traceback they are given. I commonly use `post_mortem()` in the `except` block. However, we will cover the `pm()` method since I find it to be a bit more powerful. Let's try and see how this works in practice.
>
> Open up the python REPL by typing `python` in your shell in the root of this project. From there, let's import the `main` method from the `main` module and import `pdb` as well. Play the game until the we get the exception after trying to type `Y` to continue the game.
>
> ```
> >>> import pdb
> >>> from main import main
> >>> main()
> [...]
> Would you like to play again?[Y/n]: Y
> Traceback (most recent call last):
>   File "main.py", line 12, in <module>
>     main()
>   File "main.py", line 8, in main
>     GameRunner.run()
>   File "/Users/Development/pdb-tutorial/dicegame/runner.py", line 62, in run
>     i_just_throw_an_exception()
>   File "/Users/Development/pdb-tutorial/dicegame/utils.py", line 13, in i_just_throw_an_exception
>     raise UnnecessaryError("You actually called this function...")
> dicegame.utils.UnnecessaryError: You actually called this function...
> ```
>
> 
>
> Now, let's call the `pm()` method from the `pdb` module and see what happens.
>
> ```
> >>> pdb.pm()
> > /Users/Development/pdb-tutorial/dicegame/utils.py(13)i_just_throw_an_exception()
> -> raise UnnecessaryError("You actually called this function...")
> (Pdb) 
> ```
>
> 
>
> Look at that! We recover from the point where the last exception was thrown and are placed in the `pdb` prompt. From here, we can examine the state the program was in before it crashed which will help you in your investigation.
>
> **NB**: You can also start the `main.py` script using `python -m pdb main.py` and `continue` until an exception is thrown. Python will automatically enter `post_mortem` mode at the uncaught exception.
>
> 
>
> links:https://wil.yegelwel.com/pdb-pm/
>
> #### Python Debugging: How to use pdb.pm()
>
> Python’s debugger, [pdb](https://docs.python.org/3/library/pdb.html), is a great tool and one of my favorite uses of pdb is `pdb.pm()`. This post shows how to use `pdb.pm()` by working through an example of a buggy implementation of mergesort.
>
> `pdb.pm()` launches `pdb` to debug the most recently raised exception. When you find yourself saying “Shit! That exception occurred four layers deep in code I wasn’t editting and setting up the state to reproduce this is going to take 20 minutes” or “I wish I could just debug right from where the exception was raised”, `pdb.pm()` is what you want.
>
> As a working example, consider the following implementation of mergesort. It isn’t great: the code has a logic bug, duplicates memory and suffers from array expansion, we will only focus on the logic bug.
>
> ```Python
> def mergesort(xs): 
>     if len(xs) == 1: 
>         return xs 
>     else: 
>         left = mergesort(xs[:len(xs)//2]) 
>         right = mergesort(xs[len(xs)//2:]) 
>         ys = [] 
>         left_i, right_i = 0, 0 
>         while left_i < len(left) or right_i < len(right): 
>             if left[left_i] <= right[right_i]: 
>                 ys.append(left[left_i]) 
>                 left_i += 1 
>             else: 
>                 ys.append(right[right_i]) 
>                 right_i += 1 
>         return ys 
> ```
>
> The bug is in the while loop. If `left_i` is less than `len(left)` it will drop into the loop body, even if `right_i` is greater than or equal to `len(right)`. In that case, it will try to index out of the array bounds. Gasp!
>
> If you try to use this code, you will get the error we expect.
>
> ```Python
> In [2]: mergesort([6,5,4,3,2,1])                                                                                                                                                                                  
> ---------------------------------------------------------------------------
> IndexError                                Traceback (most recent call last)
> <ipython-input-30-15081790cedb> in <module>
> ----> 1 mergesort([6,5,4,3,2,1])
> 
> <ipython-input-28-cb4e97206eb8> in mergesort(xs)
>       3         return xs
>       4     else:
> ----> 5         left = mergesort(xs[:len(xs)//2])
>       6         right = mergesort(xs[len(xs)//2:])
>       7         ys = []
> 
> <ipython-input-28-cb4e97206eb8> in mergesort(xs)
>       4     else:
>       5         left = mergesort(xs[:len(xs)//2])
> ----> 6         right = mergesort(xs[len(xs)//2:])
>       7         ys = []
>       8         left_i, right_i = 0, 0
> 
> <ipython-input-28-cb4e97206eb8> in mergesort(xs)
>       8         left_i, right_i = 0, 0
>       9         while left_i < len(left) or right_i < len(right):
> ---> 10             if left[left_i] <= right[right_i]:
>      11                 ys.append(left[left_i])
>      12                 left_i += 1
> 
> IndexError: list index out of range
> ```
>
> While we already know what the bug is, we can use `pdb.pm()` to poke at the state to see what is going on. We can query for the state of variables to see what triggered the problem.
>
> ```Python
> In [33]: import pdb; pdb.pm()                                                                                                                                                                                      
> > <ipython-input-28-cb4e97206eb8>(10)mergesort()
> -> if left[left_i] <= right[right_i]:
> (Pdb) left_i, right_i
> (0, 1)
> (Pdb) left
> [5]
> (Pdb) right
> [4]
> ```
>
> Ok, now we can see what happened. Both `left` and `right` had a single element but the code tried to index the non-existent second element in `right`.
>
> If we needed to, we could step up the traceback to understand the state using the command `u` (for up). Then we can poke at the state higher in the stack as we did before.
>
> ```Python
> (Pdb) u
> > <ipython-input-28-cb4e97206eb8>(6)mergesort()
> -> right = mergesort(xs[len(xs)//2:])
> (Pdb) left
> [6]
> (Pdb) xs
> [6, 5, 4]
> (Pdb) xs[len(xs)//2:]
> [5, 4]
> ```
>
> Debugging exceptions becomes a lot easier once you become comfortable with `pdb.pm()`.



**pycallgraph**

**graphviz**

```bash
pycallgraph graphviz script.py
```

