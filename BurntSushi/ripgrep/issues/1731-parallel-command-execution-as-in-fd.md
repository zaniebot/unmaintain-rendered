```yaml
number: 1731
title: Parallel Command Execution (as in fd)
type: issue
state: closed
author: dharrigan
labels:
  - wontfix
assignees: []
created_at: 2020-11-16T13:01:45Z
updated_at: 2020-11-16T14:08:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1731
synced_at: 2026-01-12T16:13:24Z
```

# Parallel Command Execution (as in fd)

---

_@dharrigan_

Hi,

A feature I personally find incredibly useful in [fd](https://github.com/sharkdp/fd), is the ability for it to pass the results of the run individually, or in batch, to an external program using parallel command execution, using these switches:

```
    -x, --exec <cmd>                   Execute a command for each search result
    -X, --exec-batch <cmd>             Execute a command with all search results at once
```

More details can be found here about fd's [parallel command execution](https://github.com/sharkdp/fd#parallel-command-execution).

Whilst a similar thing can be achieved by doing this already in rg (for example): 

```
vim `rg -l text`
```

It's a bit cumbersome and is only working in batch. It would be great if rg had a similar mechanism as in fd, i.e.,

`rg text -X vim {}` (for batch)

Thank you.

-=david=-



---

_Comment by @BurntSushi on 2020-11-16 14:07_

I'd rather folks relied on their shell to do this. And if you don't want batch mode, you can use `xargs -n1` to execute a command for each argument.

---

_Closed by @BurntSushi on 2020-11-16 14:07_

---

_Label `wontfix` added by @BurntSushi on 2020-11-16 14:08_

---
