```yaml
number: 2696
title: issue when using double quotes to search in linux
type: issue
state: closed
author: abhijithcd
labels: []
assignees: []
created_at: 2024-01-02T14:25:22Z
updated_at: 2024-01-02T17:28:27Z
url: https://github.com/BurntSushi/ripgrep/issues/2696
synced_at: 2026-01-12T16:13:24Z
```

# issue when using double quotes to search in linux

---

_@abhijithcd_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

Using v13.0.0.



### How did you install ripgrep?

linux-musl

### What operating system are you using ripgrep on?

CentOS 7.6,  zsh 5.0.2

### Describe your bug.

Varying behaviour when searching a string with '$' character in it.

Searching `rg -F '$finish'` and `rg '\$finish'` works fine.
Issue cases reported below.

I saw a few similar issues reported for windows but didn't see it for linux. 
Is this an expected behaviour?

### What are the steps to reproduce the behavior?

`rg "\$finish"` and  `rg -F "$finish"`

### What is the actual behavior?

Searching with`rg "\$finish"` doesn't return any results

Searching with `rg -F "$finish"` dumps a huge amount of text (content of all the files under the search path).

### What is the expected behavior?

ripgrep should return 2 matches when using double quotes as well.

---

_Comment by @s-p-turner on 2024-01-02 15:08_

This looks like expected behaviour to me. I don't believe your issue is anything to do with ripgrep. The dollar sign is a special character which Linux uses to specify an environment variable. Putting the dollar sign in double quotes (or no quotes) causes the Linux shell interpreter to substitute the name of the environment variable with its value. Specifying _\\$_ stops the shell from doing the substitution, and the literal _$_ character is passed instead. Putting the dollar sign in single quotes stops the shell interpreter from doing this environment variable detection.

So -

1. '$finish'  - passes the literal string **$finish**.
2. '\\$finish' - passes the literal string **\\$finish**.
3. "$finish"  - passes the value of the environment variable named _finish_, which will be blank if the environment variable is unassigned.
4. "\\$finish" - passes the literal string **$finish** (same as no. 1)


---

_Locked by @ghost on 2024-01-02 17:28_

---
