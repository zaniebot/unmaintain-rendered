```yaml
number: 2906
title: stdin is not searched if it is a directory
type: issue
state: closed
author: lolbinarycat
labels:
  - doc
assignees: []
created_at: 2024-09-30T03:10:51Z
updated_at: 2024-09-30T11:38:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2906
synced_at: 2026-01-12T16:13:25Z
```

# stdin is not searched if it is a directory

---

_@lolbinarycat_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

### How did you install ripgrep?

nix-inst ripgrep

https://codeberg.org/binarycat/nix-inst

### What operating system are you using ripgrep on?

NixOS 24.05 (Uakari)

### Describe your bug.

if stdin is a directory, ripgrep acts like it is a terminal

### What are the steps to reproduce the behavior?

rg a < some_dir

### What is the actual behavior?

if stdin is a directory, ripgrep acts like it is a terminal

### What is the expected behavior?

ripgrep recurses into stdin

yes this is kinda a weird request, but it's useful for quick scripts that wrap rg, with no way to pass file arguments to rg.

filing this as a bug because although i can't find any docs that say "ripgrep searches stdin unless it is a terminal", they probably should exist.

---

_Comment by @lolbinarycat on 2024-09-30 03:12_

if this is too niche to spend your time fixing, i am a rust developer and would be willing to fix this.

---

_Label `doc` added by @BurntSushi on 2024-09-30 03:17_

---

_Comment by @BurntSushi on 2024-09-30 03:25_

I don't understand the use case. You mention quick wrapper scripts, but I don't really get it. I write lots of wrapper scripts, and it's easy to forward arguments from the wrapper script to a command run by that wrapper script. 

You don't mention the expected behavior here. I guess you're expecting ripgrep to search it as if it were just a directory provided as an argument? Do other Unix commands do this? I've never seen this usage pattern before. 

The way ripgrep works is that it looks for something resembling a file on stdin to search. And if it can't find a file (which it can't in this case), it searches the current working directory. This is actually a heuristic and this heuristic has caused lots of problems because it's common for things to pass something that looks like a file on stdin even when it's not really intended. So I'm very hesitant to change that heuristic. And your suggesting here would add a third case to that heuristic. 

I think at best the docs can be improved. The man page already mentions this behavior, although I suppose it could be slightly more precise to cover this case. But I'm not even sure this is worth doing because this seems so idiosyncratic to me. Maybe you can elaborate more.

> if this is too niche to spend your time fixing, i am a rust developer and would be willing to fix this.

Note that it's not just the initial effort here. It's also the ongoing maintenance burden that stuff like this adds. Especially to what is already a finicky heuristic.

---

_Comment by @lolbinarycat on 2024-09-30 04:15_

> I don't understand the use case. You mention quick wrapper scripts, but I don't really get it. I write lots of wrapper scripts, and it's easy to forward arguments from the wrapper script to a command run by that wrapper script.

yes, unless all those arguments are already used for something (eg. constructing the regex to search with)

> You don't mention the expected behavior here. I guess you're expecting ripgrep to search it as if it were just a directory provided as an argument? 

yes, that's what "recurses into" means

> Do other Unix commands do this? I've never seen this usage pattern before.

unix commands don't typically change behavior based on the type of stdin.  they sometime do for stdout, but usually just for terminal colors or automatically invoking a pager.

usually they just crash if any stdio is a directory, not silently ignore it.

> The way ripgrep works is that it looks for something resembling a file on stdin to search. And if it can't find a file (which it can't in this case), it searches the current working directory. This is actually a heuristic and this heuristic has caused lots of problems because it's common for things to pass something that looks like a file on stdin even when it's not really intended. So I'm very hesitant to change that heuristic. And your suggesting here would add a third case to that heuristic.

I was under the impression it just checked if stdin was a pty, and if not, treats it like it was passed as an argument.

---

_Closed by @BurntSushi on 2024-09-30 11:38_

---

_Comment by @BurntSushi on 2024-09-30 11:38_

> yes, that's what "recurses into" means

Ah sorry I missed this.

> unix commands don't typically change behavior based on the type of stdin. they sometime do for stdout, but usually just for terminal colors or automatically invoking a pager.
>
> usually they just crash if any stdio is a directory, not silently ignore it.

ripgrep isn't without precedent here, but yes, one of the downsides of messing around with stdin like this is that there are indeed some cases where ripgrep will ignore what's on stdin and search the current working directory. This is why in ripgrep 14, I introduced some logging messages (that appear when you pass `--debug`) that give a little more insight into what's happening.

> I was under the impression it just checked if stdin was a pty, and if not, treats it like it was passed as an argument.

No, it's more complicated than that. Whether it's a pty or not is a minimum criteria:

https://github.com/BurntSushi/ripgrep/blob/bf63fe8f258afc09bae6caa48f0ae35eaf115005/crates/cli/src/lib.rs#L170-L250

I've pushed a wording change to the man page to cover this case.

---
