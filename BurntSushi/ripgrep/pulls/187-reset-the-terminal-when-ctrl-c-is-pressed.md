```yaml
number: 187
title: Reset the terminal when Ctrl-C is pressed
type: pull_request
state: closed
author: lambda
labels: []
assignees: []
base: master
head: ctrlc-reset-terminal
created_at: 2016-10-19T03:47:42Z
updated_at: 2016-10-30T01:55:11Z
url: https://github.com/BurntSushi/ripgrep/pull/187
synced_at: 2026-01-12T18:23:12Z
```

# Reset the terminal when Ctrl-C is pressed

---

_@lambda_

If a user hits Ctrl-C to exit out of a search in the middle of printing
a line, we don't want to leave the terminal colors screwed up for them.
Catch Ctrl-C using the ctrlc crate, obtain a stdout lock to ensure that
other threads don't continue writing after we do so, reset the terminal,
and exit the program.

Closes #119


---

_Comment by @lambda on 2016-10-19 03:54_

This PR is a bit of a strawman; I figured I'd try the simplest possible thing that seemed to work. I first tried to see whether I should thread this into `ColoredTerminal` somehow, but that seemed complicated, and this approach seemed simpler.

I haven't tested this at all on Windows, so don't know if this will work properly on there.

On my system, for some reason, this suppresses the shell prompt printed after exiting. I haven't quite figure out why that is. But it does have the effect of resetting the colors appropriately, so if I just hit "return", I do get the prompt properly. Writing out extra newlines and flushing doesn't seem to help this.

I had expected that this would have the effect of losing the information about the process being killed by a signal, just returning 1 instead, since I didn't clear the signal handler and re-raise the signal, but `echo $?` gives me 130 as you'd expect from an interrupted process. So I guess that's good, if unexpected.


---

_Comment by @BurntSushi on 2016-10-19 11:12_

@lambda Nice! Thanks so much for doing this. I'll pull this down and try to play with it soon before merging. (It may not happen until this weekend, sorry!)


---

_Comment by @BurntSushi on 2016-10-30 01:55_

OK, I tried this out and it does indeed fix the issue for me. Interestingly, I'm not experiencing the shell prompt suppression. I get my shell prompt back, but I have no idea what the forces are at play here. (FWIW, I'm running `bash` in `konsole` as my terminal emulator on Linux.)

I merged this in 811fcc1fe80e32916350888f4e6923cc2488901a (after a rebase).

Thanks so much!


---

_Closed by @BurntSushi on 2016-10-30 01:55_

---
