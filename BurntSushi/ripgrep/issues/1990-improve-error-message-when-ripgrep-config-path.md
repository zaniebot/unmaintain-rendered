```yaml
number: 1990
title: Improve error message when RIPGREP_CONFIG_PATH refers to a nonexistent file
type: issue
state: closed
author: roryokane
labels:
  - enhancement
assignees: []
created_at: 2021-09-15T02:51:49Z
updated_at: 2021-11-15T15:29:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1990
synced_at: 2026-01-12T16:13:24Z
```

# Improve error message when RIPGREP_CONFIG_PATH refers to a nonexistent file

---

_@roryokane_

## Current behavior

When `RIPGREP_CONFIG_PATH` refers to a nonexistent file, this is the warning that is currently printed:

```console
$ RIPGREP_CONFIG_PATH=~/.config/ripgreprc rg 'search regex'
/Users/roryokane/.config/ripgreprc: No such file or directory (os error 2)
```

## Proposed improvement

The error message I’d prefer:

```console
$ RIPGREP_CONFIG_PATH=~/.config/ripgreprc rg 'search regex'
Problem reading the config file specified in RIPGREP_CONFIG_PATH:
/Users/roryokane/.config/ripgreprc: No such file or directory (os error 2)
```

I wish I could fit the phrase “the RIPGREP_CONFIG_PATH environment variable” in that first line, but I couldn’t think of a wording that wouldn’t make the error distractingly long when the user doesn’t want to fix that warning soon, which might happen occasionally.

## Rationale

This would be especially useful for those who have set `RIPGREP_CONFIG_PATH` in their shell’s config, and have recently copied their shell config to a new computer. They may not remember that `RIPGREP_CONFIG_PATH` is set.

To illustrate, here’s how the current error message looks in that case:

```shell
# somewhere in my shell config
export RIPGREP_CONFIG_PATH=~/.config/ripgreprc
```

```console
$ rg 'search regex'
/Users/roryokane/.config/ripgreprc: No such file or directory (os error 2)
```

When I first saw that error, I assumed that ripgrep looked for that file by default, and I was surprised that it was so verbose about a config file not existing. It took me some searching to realize that `RIPGREP_CONFIG_PATH` was involved. After that, I solved the problem by copying my config file to the new computer.

---

_Label `enhancement` added by @BurntSushi on 2021-11-15 15:03_

---

_Closed by @BurntSushi on 2021-11-15 15:29_

---
