---
title: "Missing Semester Lecture1: Course Overview + The Shell"
date: 2024-04-15 19:09:26
tags: shell
categories:
- missing semester
---
### Lecture 1: Course Overview + The Shell

[https://missing.csail.mit.edu/2020/course-shell/](https://missing.csail.mit.edu/2020/course-shell/)

```bash
mv a.md b.md
```

> rename the file

```bash
mv a.md ../b.md
```

> move the file

```bash
rm dir1
```

> delete all items recursive

```bash
rmdir dir1
```

> delete dir only empty

```bash
ls ..
```

> list all files of the father dir

shortcut: `Control + L`

> same as command clear

```bash
cat < hello.txt > hello2.txt
```

> replace hello2.txt with the content of hello.txt

```bash
cat < hello.txt >> hello2.txt
```

> append the content of hello.txt to hello2.txt as an appendix

* **Pipe**

```bash
ls - l | tail -n1
```

> get last info of (ls -l)

```bash
curl --head --silent google.com | grep -i content-length
```

> output is 'Content-Length:219'