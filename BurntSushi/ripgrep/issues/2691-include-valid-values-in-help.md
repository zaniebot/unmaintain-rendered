```yaml
number: 2691
title: Include valid values in --help
type: issue
state: open
author: smprather
labels: []
assignees: []
created_at: 2023-12-20T00:06:00Z
updated_at: 2023-12-20T05:35:07Z
url: https://github.com/BurntSushi/ripgrep/issues/2691
synced_at: 2026-01-12T16:13:24Z
```

# Include valid values in --help

---

_@smprather_

There are many ripgrep options that take an = value. Can the help be improved to show the list of valid values? The error message should do the same. As it is now, all I get is this. Thanks @BurntSushi !

```
$ rg --color
missing value for flag --color: missing argument for option '--color'

$ rg --help  ### Edit: this should have been -h ###
...
--color=WHEN                    When to use color.
```

I had to search the repo to find the valid values.
![image](https://github.com/BurntSushi/ripgrep/assets/6503931/f1226a37-54c8-43a5-8b52-1f2f493e5863)


---

_Comment by @BurntSushi on 2023-12-20 01:03_

The values for `--color` are in the `--help` output. The output you've pasted _looks_ like it's from the `-h` output even though you have `rg --help` as the command you ran.

---

_Comment by @smprather on 2023-12-20 05:34_

You are correct. I editted the OP to correct.

I am used to -h and --help being aliases. In fact, I can't recall a case where that wasn't true.

I would still request you add the valid values to the error message. But I'll leave that up to you. I'll leave it open. Feel free to close if you want to take a pass.

Thanks for this incredible tool. I use it literally 100 times/day!

---
