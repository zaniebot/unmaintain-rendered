```yaml
number: 948
title: "use different exit codes for \"no match\" and \"error\""
type: issue
state: closed
author: Delodax
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2018-06-15T11:01:51Z
updated_at: 2018-06-19T11:41:45Z
url: https://github.com/BurntSushi/ripgrep/issues/948
synced_at: 2026-01-12T16:13:22Z
```

# use different exit codes for "no match" and "error"

---

_@Delodax_

#### What version of ripgrep are you using?

0.8.1

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS 10.13.5

#### Describe your question, feature request, or bug.

Hi!

I am using ripgrep from within a node.js program using `child_process.exec()`. I've noticed that when I don't find anyhting in a search, ripgrep terminates with a non-zero exit code thus informing `child_process.exec()` that "something went wrong".

I'd simply like to know, if there is any flag one can use or other measure to prevent a non-zero exit code during the aforementioned circumstances. I can solve the issue from with the node.js program, but would think "manipulating" the exit code would be a more elegant solution.

---

_Comment by @BurntSushi on 2018-06-15 11:08_

> I've noticed that when I don't find anyhting in a search, ripgrep terminates with a non-zero exit code

This is intended behavior. ripgrep continues this very long held tradition from grep, and I don't see myself changing it or providing a flag to change it.

One place where ripgrep could probably use some improvement is in refining its exit codes. Namely, I think ripgrep only ever produces `0` or `1` as an exit code, but it would be nice to reserve `1` for "no match" and use some other exit code (probably `2`) in the case of a real error. This is what GNU grep does.

---

_Renamed from "Exit code musing" to "use different exit codes for "no match" and "error"" by @BurntSushi on 2018-06-15 11:08_

---

_Label `enhancement` added by @BurntSushi on 2018-06-15 11:08_

---

_Comment by @Delodax on 2018-06-15 11:12_

Thanks for the quick feedback. Your solution wrt extending the breadth of exit codes sounds great. 👍 

---

_Comment by @BurntSushi on 2018-06-15 11:19_

Interestingly, it looks like this might be border-line trivial to fix. This is ripgrep's current `main` function:

https://github.com/BurntSushi/ripgrep/blob/15fa77cdb384e013165687f361073defa02d9d98/src/main.rs#L56-L66

I think all that needs to happen is to change the error case to `process::exit(2)`.

There is still the matter of writing a test and updating/adding relevant docs, which might be a bit more work since I can't remember if the test harness tries to control the exit status, but either way, shouldn't be too bad.

I can patch this myself when I get a chance, but this does seem like an easy task that anyone could take on if they wanted to!

---

_Label `help wanted` added by @BurntSushi on 2018-06-15 11:19_

---

_Closed by @BurntSushi on 2018-06-19 11:41_

---
