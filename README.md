# How to use `git bisect`

Find the change that introduced a bug in your code.

The `git bisect` command helps you to find which change introduced a bug in your code. It’s easy and quick, but most people don’t know about it.

## The problem

You notice that in the most recent commit (let’s say `4a4e`), a feature is not working, and you remember than at some point in the past (let’s say commit `f8a4`) is was working just fine. The task is to find out which commit introduced the bug.

## How does it work?

Git uses the bisection algorithm to help you search the offending commit. To start, you need to mark a `bad` commit and a `good` commit, git will checkout a commit in the middle for you to test. Then you mark it either as `good` or `bad`, and then the process starts again.


| Step 1   | Step 2   | Step 3   | Step 4   | Step 5   | Step 6   | Step 7   |
|----------|----------|----------|----------|----------|----------|----------|
| `4a4e` :anguished: |     |          |          |          |          |          |
| `cfaf`   |          |          |          |          |          |          |
| `e6f1`   |          |          |          |          |          |          |
| `77ca`   |          |          |          |          |          |          |
| `1642`   |          |          |          |          |          |          |
| `4ba6`   |          |          |          |          |          |          |
| `7e56`   |          |          |          |          |          |          |
| `397b`   |          |          |          |          |          |          |
| `7eb5`   |          |          |          |          |          |          |
| `4203`   |          |          |          |          |          |          |
| `09af`   |          |          |          |          |          |          |
| `6d3b`   |          |          |          |          |          |          |
| `0219`   |          |          |          |          |          |          |
| `34a1` ? | `34a1` :anguished: | `34a1` :anguished: |          |          |          |          |
| `1054`   | `1054`   | `1054`   |          |          |          |          |
| `bcab`   | `bcab`   | `bcab`   |          |          |          |          |
| `4ae6`   | `4ae6`   | `4ae6` ? | `4ae6` :anguished: |          |          |          |
| `7d75`   | `7d75`   | `7d75`   | `7d75`   |          |          |          |
| `e079`   | `e079`   | `e079`   | `e079` ? | `e079` :anguished: | `e079` :anguished: | `e079` :sunglasses: |
| `8013`   | `8013`   | `8013`   | `8013`   | `8013` ? | `801e` :smiley: |          |
| `7487`   | `7487` ? | `7487` :smiley: | `7487` :smiley: | `7487` :smiley: |         |          |
| `f3b2`   | `f3b2`   |          |          |          |          |          |
| `b12a`   | `b12a`   |          |          |          |          |          |
| `2dac`   | `2dac`   |          |          |          |          |          |
| `1ed8`   | `1ed8`   |          |          |          |          |          |
| `f8a4` :smiley: | `f8a4` :smiley: |          |          |          |          |          |
| `...`    |          |          |          |          |          |          |  


## Demo

To start the process, just invoke `git bisect start` and mark the commit as `bad`, and mark a commit that was working `f8a4` as `good`.

```bash
(4a4e) $ git bisect start
(4a4e) $ git bisect bad
(4a4e) $ git bisect good f8a4

Bisecting: 7 revisions left to test after this (roughly 3 steps)
[cae5ba2ea747fc29dd234bb0ae51d8ad79096153] Rename function

(cae5) $ node add.js 1 2 3  // 6
(cae5) $  git bisect good

Bisecting: 7 revisions left to test after this (roughly 3 steps)
[cae5ba2ea747fc29dd234bb0ae51d8ad79096153] Rename function

# 3 or 4 steps later ...

$ git bisect bad

5e3c5e2e94836a2571a88e89f019ffc65a00293e is the first bad commit
commit 5e3c5e2e94836a2571a88e89f019ffc65a00293e
Author: Pablo Navarro Castillo <pnavarrc@gmail.com>
Date:   Sat Apr 16 11:02:21 2016 -0300

    read all input arguments

:100755 100755 f1110f6f59f0d9bd5e9db8513bd5330d44831ff9 d7fc70346dac9ebce564bef4e82f9fa78a819902 M	add.js

$ git reset
```
