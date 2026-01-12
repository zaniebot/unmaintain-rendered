```yaml
number: 270
title: name property not used.
type: issue
state: closed
author: XAMPPRocky
labels:
  - C-bug
  - A-builder
  - A-parsing
assignees: []
created_at: 2015-09-22T18:23:57Z
updated_at: 2018-08-02T03:29:45Z
url: https://github.com/clap-rs/clap/issues/270
synced_at: 2026-01-12T16:14:08Z
```

# name property not used.

---

_@XAMPPRocky_

When I was checking the help, and version output, and I realised that the name being printed is the not the name used in the yaml file. It seems to be using my `Cargo.toml` name instead.


---

_Referenced in [clap-rs/clap#239](../../clap-rs/clap/issues/239.md) on 2015-09-22 18:35_

---

_Label `C: app` added by @sru on 2015-09-22 19:12_

---

_Label `C: parsing` added by @sru on 2015-09-22 19:12_

---

_Label `T: bug` added by @sru on 2015-09-22 19:12_

---

_Label `P3: want to have` added by @sru on 2015-09-22 19:12_

---

_Comment by @kbknapp on 2015-09-22 19:17_

This is by design, unless I'm misunderstanding.

The name set in the YAML `name` field file is only used if the OS cannot determine the binary file. You can override this using the `bin_name` property to forcibly set the `name` to anything you'd like regardless of what the produced binary is called.


---

_Closed by @kbknapp on 2015-09-23 14:43_

---
