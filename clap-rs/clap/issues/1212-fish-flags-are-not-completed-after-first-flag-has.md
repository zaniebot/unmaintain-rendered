```yaml
number: 1212
title: "[fish] Flags are not completed after first flag has been completed"
type: issue
state: closed
author: zx8
labels: []
assignees: []
created_at: 2018-03-18T12:58:39Z
updated_at: 2018-08-02T03:30:20Z
url: https://github.com/clap-rs/clap/issues/1212
synced_at: 2026-01-12T16:14:10Z
```

# [fish] Flags are not completed after first flag has been completed

---

_@zx8_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* `rustc 1.24.1 (d3ae9a9e0 2018-02-27)`

### Affected Version of clap

* `v2.31.1`

### Expected Behavior Summary
```
$ rg --<TAB>
# list of completions
$ rg --no-follow --<TAB>
# list of completions
```

### Actual Behavior Summary
```
$ rg --<TAB>
# works - list of completions
$ rg --no-follow --<TAB>
# broken; no completions
```


---

_Comment by @zx8 on 2018-03-18 13:06_

The issue here is the usage of the `__fish_using_command` function. Here's what's going on:

```
$ rg --no-follow --<TAB>
function __fish_using_command
    # cmd is set to array: ["rg", "--no-line-number"]
    set cmd (commandline -opc)
    # 'count $cmd' is 2, 'count $argv' is 1  
    if [ (count $cmd) -eq (count $argv) ]
        # etc.
        return 0
    end
    # no completions
    return 1
end
```

If you drop this function entirely (which I understand was introduced to fix subcommand completion; https://github.com/kbknapp/clap-rs/pull/687, /ping @wdv4758h), flag completions >1 work again, i.e.
```
complete -c rg -s N -l no-line-number -d 'Suppress line numbers.'
```

---

_Referenced in [clap-rs/clap#1214](../../clap-rs/clap/pulls/1214.md) on 2018-03-18 16:34_

---

_Closed by @kbknapp on 2018-03-19 17:13_

---

_Comment by @wdv4758h on 2018-03-19 17:34_

this issue fix so fast, nice :+1: 

---

_Comment by @kbknapp on 2018-03-19 18:03_

I'm putting out the new version in just a minute :)

---

_Referenced in [BurntSushi/ripgrep#862](../../BurntSushi/ripgrep/issues/862.md) on 2018-03-19 18:50_

---
