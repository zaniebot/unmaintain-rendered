```yaml
number: 2721
title: multiline search regex wildcard not expanding
type: issue
state: closed
author: Gooberpatrol66
labels:
  - invalid
assignees: []
created_at: 2024-01-25T12:08:37Z
updated_at: 2024-01-25T13:26:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2721
synced_at: 2026-01-12T16:13:24Z
```

# multiline search regex wildcard not expanding

---

_@Gooberpatrol66_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

13.0.0

### How did you install ripgrep?

guix

### What operating system are you using ripgrep on?

guix

### Describe your bug.

ripgrep is searching for the regex literally

### What are the steps to reproduce the behavior?
```
$ rg --multiline '<<<<<<< HEAD.*>>>>>>>'

$ cat test
<<<<<<< HEAD
A
=======
B
>>>>>>>
```


### What is the actual behavior?

```
scripts/git-hooks/dont-commit-with-merge-conflicts.sh
3:#when https://issues.guix.gnu.org/67450 fix is upstreamed, replace below with git diff | grep -P '<<<<<<< HEAD(.|\n)*>>>>>>>'
13:#git diff $against | tr '\n' '\a' | grep --color=always '<<<<<<< HEAD.*=======.*>>>>>>>' | tr '\a' '\n'
14:grep -r --color=always '<<<<<<< HEAD.*=======.*>>>>>>>' | tr '\a' '\n'
- 
```


### What is the expected behavior?

it finds file test

---

_Comment by @BurntSushi on 2024-01-25 13:26_

As in virtually every regex engine, the `.` in ripgrep's regex engine matches any character _except_ a newline. Notice how one of your grep commands uses `(.|\n)`.

There are many ways to achieve your goal:

```
$ rg --multiline '<<<<<<< HEAD.*>>>>>>>'
$ rg --multiline --multiline-dotall '<<<<<<< HEAD.*>>>>>>>'
test
1:<<<<<<< HEAD
2:A
3:=======
4:B
5:>>>>>>>
$ rg --multiline '<<<<<<< HEAD(?s:.)*>>>>>>>'
test
1:<<<<<<< HEAD
2:A
3:=======
4:B
5:>>>>>>>
$ rg --multiline '<<<<<<< HEAD\p{any}*>>>>>>>'
test
1:<<<<<<< HEAD
2:A
3:=======
4:B
5:>>>>>>>
```

---

_Closed by @BurntSushi on 2024-01-25 13:26_

---

_Label `invalid` added by @BurntSushi on 2024-01-25 13:26_

---
