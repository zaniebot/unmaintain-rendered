```yaml
number: 3223
title: Numerous “No such file or directory (os error 2)” messages
type: issue
state: closed
author: dbitouze
labels: []
assignees: []
created_at: 2025-11-19T20:49:36Z
updated_at: 2025-11-20T11:26:05Z
url: https://github.com/BurntSushi/ripgrep/issues/3223
synced_at: 2026-01-12T16:13:25Z
```

# Numerous “No such file or directory (os error 2)” messages

---

_@dbitouze_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

15.1.0 (rev af60c2de9d)

### How did you install ripgrep?

`ripgrep` installed with `ripgrep-15.1.0-x86_64-unknown-linux-musl.tar.gz`

### What operating system are you using ripgrep on?

Linux Mageia 9

### Describe your bug.

In stdout, numerous “No such file or directory (os error 2)” messages appear mixed in with the relevant `rg` results.


### What are the steps to reproduce the behavior?

Run in the terminal such an innocuous command as:

`rg -F "lst@definelanguage[maths]" -g "lstlang0.sty" ~/`

### What is the actual behavior?

I get in stdout the relevant `rg` results but they are mixed with a lot of garbage of the form:

`rg: /home/⟨path to an (indeed) non-existent file⟩: No such file or directory (os error 2)`

On the other hand, if I collect the result in a file:

`rg -F "lst@definelanguage[maths]" -g "lstlang0.sty" ~/ > ~/rg.txt`

this file contains only the relevant `rg` results.

### What is the expected behavior?

I would expect only the the relevant `rg` results in stdout.

---

_Comment by @BurntSushi on 2025-11-19 21:21_

Please provide an MRE. Without instructions to reproduce, this won't be actionable.

---

_Comment by @dbitouze on 2025-11-20 08:25_

> Please provide an MRE. Without instructions to reproduce, this won't be actionable.

Sure, but I don't know how to built such a MRE.

---

_Comment by @BurntSushi on 2025-11-20 10:01_

You'll probably need to debug this on your own then. My guess is that this isn't a problem with ripgrep specifically.

In any case, there are a number of things you could try. For example, the command you've presented searches your entire home directory. Why not try searching something smaller to see if you can isolate the problem?

---

_Comment by @dbitouze on 2025-11-20 10:12_

> You'll probably need to debug this on your own then. My guess is that this isn't a problem with ripgrep specifically.

Indeed, probably not a `ripgrep` issue per se (sorry for the noise in this case).

> In any case, there are a number of things you could try. For example, the command you've presented searches your entire home directory. Why not try searching something smaller to see if you can isolate the problem?

If I restrict the search to a subdirectory of my home directory, the problem remains but only the (indeed) non-existent files of this subdirectory are mentioned.

---

_Comment by @jafd on 2025-11-20 10:22_

Are you 100% sure the error messages indeed go to stdout and not to stderr? And you have no idea whatsoever where the nonexistent filenames are coming from?

---

_Comment by @dbitouze on 2025-11-20 10:59_

> Are you 100% sure the error messages indeed go to stdout and not to stderr?

Well, I guess you're right: the error messages go to stderr.

>  And you have no idea whatsoever where the nonexistent filenames are coming from?

No. If I'm right, these files were removed not at the same time.


---

_Comment by @jafd on 2025-11-20 11:20_

I then don't think it's a bug at all. 

When you read a directory and then act on its files, and some other process is racing you creating and deleting files at the same time, TOCTOU problems are pretty much unavoidable. It doesn't matter if you read the whole directory at once and then act on its files, or you act on each file as you are reading the directory; as long as there are two steps to it, races are possible and _will_ happen. 

---

_Comment by @dbitouze on 2025-11-20 11:26_

OK, thank you very much for your insightful comments!

---

_Closed by @dbitouze on 2025-11-20 11:26_

---
