```yaml
number: 2455
title: replacements - $1 not recognized if followed by underscore
type: issue
state: closed
author: brunetton
labels:
  - duplicate
assignees: []
created_at: 2023-03-15T00:18:26Z
updated_at: 2023-03-16T15:23:56Z
url: https://github.com/BurntSushi/ripgrep/issues/2455
synced_at: 2026-01-12T16:13:24Z
```

# replacements - $1 not recognized if followed by underscore

---

_@brunetton_

#### What version of ripgrep are you using?

ripgrep 13.0.0

#### How did you install ripgrep?

Debian

#### What operating system are you using ripgrep on?

Ubuntu 22.04

#### Describe your bug.

Can't use `$1` in replacements when followed by underscore

#### What are the steps to reproduce the behavior?

`echo "hello" | rg '(.*)' -r '--$1_--'`

#### What is the actual behavior?

```
DEBUG|rg::config|crates/core/config.rs:40: /home/bruno/.ripgreprc: arguments loaded from config file: ["--no-line-number"]
DEBUG|rg::args|crates/core/args.rs:543: final argv: ["rg", "--no-line-number", "(.*)", "-r", "--$1_--", "--debug"]
DEBUG|globset|/usr/share/cargo/registry/ripgrep-13.0.0/debian/cargo_registry/globset-0.4.8/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
----
```

#### What is the expected behavior?

Output should be: `--hello_--`


---

_Comment by @BurntSushi on 2023-03-15 00:48_

Duplicate of #2201.

Use `${1}` instead. Otherwise the `_` is treated as part of the capture group reference.

---

_Closed by @BurntSushi on 2023-03-15 00:48_

---

_Label `duplicate` added by @BurntSushi on 2023-03-15 00:48_

---

_Comment by @brunetton on 2023-03-16 15:23_

Sorry for the duplicate !

---
