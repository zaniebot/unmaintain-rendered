```yaml
number: 1264
title: "-j doesn't work for single large file."
type: issue
state: closed
author: leojay
labels:
  - question
assignees: []
created_at: 2019-04-22T19:56:33Z
updated_at: 2024-04-09T22:07:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1264
synced_at: 2026-01-12T16:13:23Z
```

# -j doesn't work for single large file.

---

_@leojay_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 973de50c9e)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

I downloaded the binary from the github release page.

#### What operating system are you using ripgrep on?

Ubuntu 18.04

#### Describe your question, feature request, or bug.

I use rg to search a huge file (>20GB). And from "top" command, the cpu usage of rg is always <=100%, which means that rg is using only 1 core, instead of multiple cores.
"-j 1" and "-j 6" don't make any difference in terms of cpu usage and running time.
My workstation has 6c/12t cpu, 64GB of ram and nvme ssd. So I don't think the hardware is the restriction here. 

Does ripgrep support multithreading in single large file at all? If not, is it on the roadmap?

Thanks!
Leo

---

_Comment by @BurntSushi on 2019-04-22 19:58_

> Does ripgrep support multithreading in single large file at all?

Nope.

> If not, is it on the roadmap?

Nope. It significantly complicates the implementation.

---

_Closed by @BurntSushi on 2019-04-22 19:58_

---

_Label `question` added by @BurntSushi on 2019-04-22 19:58_

---

_Comment by @roycewilliams on 2024-04-09 21:33_

Since this is a semi-FAQ item, and searches for this question lead to this issue... 

A viable "multithreaded ripgrep search of a single very large file" workaround, _external_ to ripgrep, is to use GNU Parallel with its `--pipepart -a [inputfile]` option.

This will spawn multiple `rg` processes, and pass one raw *subsection* of `inputfile` to each process. You can adjust the number of blocks/processes to optimize for your hardware, including a `--block -1` option that spawns [core-count] processes to automaticallly divide the work into [core-count] chunks (which may be overkill, and bottlenecked on I/O).

`parallel --pipepart --block -1 -a haystackfile "rg needlestring"`

Note also that this workaround is specific to searching _single_ files. ripgrep handles the multiple-file case quite efficiently on its own!

(And for the record, I 100% support the idea that adding single-file multithreaded search to ripgrep would add complexity that @BurntSushi is perfectly justified in avoiding! I'm hoping this workaround will help others, but if it's not a net win to drop this here, let me know and I will delete it)

---
