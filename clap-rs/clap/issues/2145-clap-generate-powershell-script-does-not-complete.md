```yaml
number: 2145
title: clap_generate PowerShell script does not complete case sensitive options like -S. Only completes -s
type: issue
state: closed
author: jfishe
labels:
  - C-bug
  - E-medium
  - A-completion
  - ":money_with_wings: $5"
assignees: []
created_at: 2020-09-27T22:50:49Z
updated_at: 2023-07-05T15:36:59Z
url: https://github.com/clap-rs/clap/issues/2145
synced_at: 2026-01-12T16:14:12Z
```

# clap_generate PowerShell script does not complete case sensitive options like -S. Only completes -s

---

_@jfishe_

### Make sure you completed the following tasks

- [X] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] Searched the closed issues

### Code

https://github.com/clap-rs/clap/blob/3e9ee86713b5c407b50ba76f30cffaed25952063/clap_generate/src/generators/shells/powershell.rs

### Steps to reproduce the issue

N/A

### Version

name = "clap"
version = "2.33.1"
source = "registry+https://github.com/rust-lang/crates.io-index"
checksum = "bdfa80d47f954d53a35a64987ca1422f495b8d6483c0fe9f7117b36c2a792129"
dependencies = [
 "bitflags",
 "strsim",
 "textwrap",
 "unicode-width",
]

### Actual Behavior Summary

https://github.com/BurntSushi/ripgrep generated _rg.ps1 does not complete -S correctly. It only completes -s.

### Expected Behavior Summary

See https://github.com/BurntSushi/ripgrep/issues/1690 for details.

`.  _rg.ps1`

`rg -s<Ctrl-Space>` completes `rg -s`. It should produce:

```
rg -s
s   S

Search case sensitively (default).
```
A simple fix would be to change ListItemText for 'S' to 'S '. The space makes ListItemText unique, which is all that's needed.

### Additional context

A simple fix may be to test for upper case and append a space. I don't know rust, so a PR is beyond me.

https://github.com/clap-rs/clap/blob/3e9ee86713b5c407b50ba76f30cffaed25952063/clap_generate/src/generators/shells/powershell.rs#L94 and https://github.com/clap-rs/clap/blob/3e9ee86713b5c407b50ba76f30cffaed25952063/clap_generate/src/generators/shells/powershell.rs#L105

For example, change the second '{}' to '{} ' in the format! below after adding a check for upper case. Sorry I can't be more helpful.

```
if let Some(short_aliases) = option.get_visible_short_aliases() {
                for data in short_aliases {
                    completions.push_str(&preamble);
                    completions.push_str(
                        format!(
                            "'-{}', '{} ', {}, '{}')",
                            data, data, "[CompletionResultType]::ParameterName", tooltip
                        )
                        .as_str(),
                    );
                }
            }
        }
```

### Debug output

N/A

---

_Label `T: bug` added by @jfishe on 2020-09-27 22:50_

---

_Label `C: generator` added by @pksunkara on 2020-09-28 08:05_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-10-25 20:02_

---

_Referenced in [clap-rs/clap#3022](../../clap-rs/clap/issues/3022.md) on 2021-11-14 01:52_

---

_Referenced in [clap-rs/clap#1232](../../clap-rs/clap/issues/1232.md) on 2021-11-15 14:31_

---

_Referenced in [epage/clapng#92](../../epage/clapng/issues/92.md) on 2021-12-06 17:34_

---

_Referenced in [epage/clapng#165](../../epage/clapng/issues/165.md) on 2021-12-06 20:18_

---

_Referenced in [epage/clapng#238](../../epage/clapng/issues/238.md) on 2021-12-06 22:24_

---

_Label `E-medium` added by @epage on 2021-12-13 15:03_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2021-12-13 17:55_

---

_Referenced in [clap-rs/clap#4992](../../clap-rs/clap/pulls/4992.md) on 2023-07-04 07:24_

---

_Closed by @epage on 2023-07-05 15:36_

---
