```yaml
number: 1044
title: List matching filenames case insensitively
type: issue
state: closed
author: HaleTom
labels:
  - question
assignees: []
created_at: 2018-09-08T13:59:08Z
updated_at: 2018-09-08T14:25:24Z
url: https://github.com/BurntSushi/ripgrep/issues/1044
synced_at: 2026-01-12T16:13:22Z
```

# List matching filenames case insensitively

---

_@HaleTom_

#### What version of ripgrep are you using?

```
ripgrep 0.9.0 (rev e86d3d95c2)
-SIMD -AVX
```

#### How did you install ripgrep?

`pacman -S ripgrep`

#### What operating system are you using ripgrep on?

`Linux svelte 4.16.18-1-MANJARO #1 SMP PREEMPT Tue Jun 26 15:27:59 UTC 2018 x86_64 GNU/Linux`

#### Describe your question, feature request, or bug.

I was trying to search for a filename using:

```
rg -g '*<text>*' --files
```

I wasn't finding a match of a file that I knew to exist - because of the case of the text.

My work-around was to fall back to: `ag -g '.*<text>.*'`

Would you consider implementing regex matches on the whole pathname of a file?

---

_Comment by @BurntSushi on 2018-09-08 14:24_

If you want case insensitive globbing, then you can use `--iglob`.

If you want to apply a regex to file paths, then use shell pipelines. e.g., `rg --files | rg -i '<text>'`. This can even be bundled into an alias, `alias rgg="rg --files | rg -i"` and then `rgg '<text>'` does what you want. A benefit of using shell pipelines is that the specific matching part of each file name will be colored when printed to a terminal.

Alternatively, you might consider using [`fd`](https://github.com/sharkdp/fd), which is a tool designed for listing files. It respects your `.gitignore` by default just like ripgrep.

---

_Closed by @BurntSushi on 2018-09-08 14:24_

---

_Label `question` added by @BurntSushi on 2018-09-08 14:24_

---
