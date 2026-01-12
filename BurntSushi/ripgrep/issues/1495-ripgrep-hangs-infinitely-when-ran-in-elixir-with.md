```yaml
number: 1495
title: ripgrep hangs infinitely when ran in Elixir with System.cmd
type: issue
state: closed
author: zapateo
labels:
  - invalid
assignees: []
created_at: 2020-02-21T16:07:36Z
updated_at: 2020-02-21T16:28:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1495
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep hangs infinitely when ran in Elixir with System.cmd

---

_@zapateo_

I have a problem running `rg` from the Elixir programming language.
I cannot understand if the problems is related to Elixir or to ripgrep; I decided to post the issue here because if I change the arguments passed to the `rg` command everything works fine.

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
sudo dnf install ripgrep
```

#### What operating system are you using ripgrep on?

Fedora 31 Workstation

#### Describe your question, feature request, or bug.

If I run `rg` using the function [System.cmd](https://hexdocs.pm/elixir/System.html#cmd/3) in the Elixir programming language, the execution of `rg` never ends.

#### If this is a bug, what are the steps to reproduce the behavior?

1. Open the Elixir shell with `iex`
2. Execute `System.cmd("rg", ["mypattern"])`
3. The process never ends.

The problem seems to solve if the PATH is passed as argument to `rg`:

1. Open the Elixir shell with `iex`
2. Execute `System.cmd("rg", ["mypattern", "."])`
3. Elixir prints the output of the command.

#### If this is a bug, what is the actual behavior?

```
$ iex
iex> System.cmd("rg", ["mypattern", "--debug"])
DEBUG|grep_regex::literal|/usr/share/cargo/registry/grep-regex-0.1.4/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(mypattern)], limit_size: 250, limit_class: 10 }
                   DEBUG|globset|/usr/share/cargo/registry/globset-0.4.4/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```
And then it hangs forever.

#### If this is a bug, what is the expected behavior?

I expected `rg` to show the same result I would get if I have explicitly passed the PATH, as it does with the shell, not to hang infinitely.

---

_Comment by @BurntSushi on 2020-02-21 16:23_

ripgrep attempts to read stdin if no path arguments are given and it cannot detect if it's on a tty. So yes, please provide a path to ripgrep to eliminate the need for ripgrep to guess.

---

_Closed by @BurntSushi on 2020-02-21 16:23_

---

_Label `invalid` added by @BurntSushi on 2020-02-21 16:23_

---

_Comment by @zapateo on 2020-02-21 16:28_

Perfect, thanks for your reply.

---
