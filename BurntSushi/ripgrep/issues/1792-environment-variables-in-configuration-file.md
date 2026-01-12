```yaml
number: 1792
title: Environment variables in configuration file
type: issue
state: closed
author: yannickperrenet
labels:
  - duplicate
  - wontfix
assignees: []
created_at: 2021-01-31T17:20:28Z
updated_at: 2021-01-31T17:33:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1792
synced_at: 2026-01-12T16:13:24Z
```

# Environment variables in configuration file

---

_@yannickperrenet_

#### Describe your feature request
Being able to use environment variables in the configuration file specified by `RIPGREP_CONFIG_PATH`.

This allows for the use of `--no-config` to disable the configuration, whereas setting `alias rg = rg --ignore-file=...` would be unaffected. Then there is `--no-ignore-files` to disable the `--ignore-file` from the alias, but has the unwanted side effect of ignoring all `ignore-files`.

##### Example
Lets say the `rc` file specified by `RIPGREP_CONFIG_PATH` is as follows:
```
--ignore-file=$XDG_CONFIG_HOME/ripgrep/ignore
```

Then (running with `--debug`) shows (as indicated by "Each line is given to ripgrep as a single command line argument verbatim." from the `GUIDE.md`):
```
arguments loaded from config file: ["--ignore-file=$XDG_CONFIG_HOME/ripgrep/ignore"]
```
And so running `rg` rightfully outputs:
```
$XDG_CONFIG_HOME/ripgrep/ignore: No such file or directory (os error 2)
```

---

_Comment by @BurntSushi on 2021-01-31 17:32_

Duplicate of #931. See also, #1548.

> This allows for the use of `--no-config` to disable the configuration, whereas setting `alias rg = rg --ignore-file=...` would be unaffected. Then there is `--no-ignore-files` to disable the `--ignore-file` from the alias, but has the unwanted side effect of ignoring all `ignore-files`.

If `rg` is an alias, then `\rg` will run the main `rg` executable without the alias.

---

_Closed by @BurntSushi on 2021-01-31 17:32_

---

_Label `duplicate` added by @BurntSushi on 2021-01-31 17:33_

---

_Label `wontfix` added by @BurntSushi on 2021-01-31 17:33_

---
