```yaml
number: 2381
title: "The `--pretty` option doesn't override config file"
type: issue
state: closed
author: mrjoel
labels:
  - bug
assignees: []
created_at: 2022-12-29T22:40:39Z
updated_at: 2023-11-21T23:39:36Z
url: https://github.com/BurntSushi/ripgrep/issues/2381
synced_at: 2026-01-12T16:13:24Z
```

# The `--pretty` option doesn't override config file

---

_@mrjoel_

#### What version of ripgrep are you using?
```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
Installed from Debian bookworm

#### What operating system are you using ripgrep on?

Debian

#### Describe your bug.

I use `--no-heading \n --no-line-number` in my config file referenced by `RIPGREP_CONFIG_PATH`. I can explicitly add the `--heading` etc. options for a one-off commandline execution, but `--pretty` doesn't work as expected. I'd expect to have it be a true "convenience alias" as documented.

Instead, the output continues to use the options from the config file, including neither heading nor line numbers.

---

_Comment by @BurntSushi on 2022-12-29 23:23_

Smaller demonstration without using a config file:

```
$ rg 'fn line_buffer' --no-heading --no-line-number
crates/searcher/src/searcher/mod.rs:    fn line_buffer(&self) -> LineBuffer {
$ rg 'fn line_buffer' --no-heading --no-line-number -p
crates/searcher/src/searcher/mod.rs:    fn line_buffer(&self) -> LineBuffer {
```

This _may_ be difficult to fix, because the arg parser only permits overriding flags _of the same name_ based on position. Technically the arg parser exposes APIs to generalize this to allow arbitrary flags to override others, but it's a significant complication. I'm not sure when or if this will get fixed unfortunately.

---

_Label `bug` added by @BurntSushi on 2022-12-29 23:23_

---

_Comment by @BurntSushi on 2022-12-29 23:32_

Out of curiosity, why do you have `--no-heading` and `--no-line-number` in your config file? They seem somewhat odd things to disable by default. Especially since ripgrep will automatically disable those options (and color) when you output to something that isn't a tty. For example, `rg foo | cat`.

---

_Comment by @mrjoel on 2022-12-30 00:15_

> Out of curiosity, why do you have --no-heading and --no-line-number in your config file?

I'm just getting back to using `rg` on a regular basis. I have `--no-heading` for vertical space compactness, and `--no-line-number` mainly for eye muscle memory reading compatibility with regular grep output, but also stand-in compatibility for other scripts I have which parse `grep` output.

I have a strong preference for the vertical space compactness, and soft preference for no line number. Somewhat ironically, I like the direct TTY color output distinction for line number, and miss it when piping to `less`, and was trying to create a shell alias to include the `--pretty` option when I ran into this issue. It's not horribly inconvenient to use the spelled out version instead, just caught my off guard.

---

_Comment by @BurntSushi on 2022-12-30 12:50_

> but also stand-in compatibility for other scripts I have which parse `grep` output.

For this part at least, when ripgrep is redirected to a non-tty, it should "devolve" to standard grep format. This includes dropping line numbers and even file names when searching stdin (or a single file).

---

_Closed by @BurntSushi on 2023-11-21 23:39_

---
