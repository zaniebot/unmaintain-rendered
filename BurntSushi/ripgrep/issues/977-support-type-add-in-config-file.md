```yaml
number: 977
title: Support --type-add in config file
type: issue
state: closed
author: vbauerster
labels:
  - invalid
assignees: []
created_at: 2018-07-12T09:16:46Z
updated_at: 2018-07-12T09:21:25Z
url: https://github.com/BurntSushi/ripgrep/issues/977
synced_at: 2026-01-12T16:13:22Z
```

# Support --type-add in config file

---

_@vbauerster_

My `RIPGREP_CONFIG_PATH` consist of:

`--type-add foo:*.foo`

Why `rg` doesn't support this flag in config file?
```
error: Found argument '--type-add foo:*.foo' which wasn't expected, or isn't valid in this context
        Did you mean --type-add?
```

---

_Comment by @BurntSushi on 2018-07-12 09:20_

Please read the [documentation](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file) on the config file format:

> When we use a flag that has a value, we either put the flag and the value on the same line but delimited by an = sign (e.g., --max-columns=150), or we put the flag and the value on two different lines. This is because ripgrep's argument parser knows to treat the single argument --max-columns=150 as a flag with a value, but if we had written --max-columns 150 in our configuration file, then ripgrep's argument parser wouldn't know what to do with it.

---

_Closed by @BurntSushi on 2018-07-12 09:20_

---

_Label `invalid` added by @BurntSushi on 2018-07-12 09:21_

---
