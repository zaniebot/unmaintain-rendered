```yaml
number: 937
title: "empty pattern doesn't display utf8"
type: issue
state: closed
author: jbolila
labels:
  - bug
assignees: []
created_at: 2018-06-03T13:13:52Z
updated_at: 2018-08-20T11:10:22Z
url: https://github.com/BurntSushi/ripgrep/issues/937
synced_at: 2026-01-12T16:13:22Z
```

# empty pattern doesn't display utf8

---

_@jbolila_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

`cargo install ripgrep -f`

#### What operating system are you using ripgrep on?

Linux Debian 9.

#### Describe your question, feature request, or bug.

On empty patterns utf8 strings aren't presented with the correct encode.

#### If this is a bug, what are the steps to reproduce the behavior?

```bash
❯ rg -S -n --color=always '' Cargo.toml
1:[package]
2:authors = ["Jo[0mo Bolila <j**>"]
..
❯ rg -S -n --color=always 'au' Cargo.toml
2:authors = ["João Bolila <j**>"]
```

Or with any utf8 content.

https://github.com/lotabout/skim/issues/83

#### If this is a bug, what is the actual behavior?

Doesn't display correctly on empty patterns

#### If this is a bug, what is the expected behavior?

Display always like when has a pattern match.

Thanks!

---

_Comment by @BurntSushi on 2018-06-03 14:06_

Initially, I couldn't reproduce this:

```
$ cat test1
ã
$ rg --color always '' test1
1:ã
```

On a hunch, I tried a different terminal emulator (specifically, `xterm`):

```
$ rg --color always '' test1
1:��
```

So that did the trick.

The specific problem here is that you're searching with an empty pattern, which matches at *every* position, even between UTF-8 code units. I suspect this itself might actually be a bug in the regex engine. A simple fix on ripgrep's side of things would be to simply not emit color escapes for zero width matches, since those color escapes aren't visually observable by humans. This would fix things because the reason why you're seeing bad output is presumably because there is a color escape sequence between UTF-8 code units, and your terminal emulator chokes on it. However, some folks are using ripgrep's color escapes to determine match locations, so I don't think I can make that change until ripgrep has a structured output feature.

So, you're pretty much stuck with this behavior for now. You might re-visit why it is you're feeding an empty pattern to ripgrep if you're looking for a workaround.

---

_Label `bug` added by @BurntSushi on 2018-06-03 14:06_

---

_Comment by @BurntSushi on 2018-06-03 14:32_

I filed a bug against the regex engine. Fixing that would also fix this bug. https://github.com/rust-lang/regex/issues/484

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
