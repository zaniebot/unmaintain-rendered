---
number: 532
title: "The load_yaml!() macro should not be relative to the working directory"
type: issue
state: closed
author: Lindenk
labels: []
assignees: []
created_at: 2016-06-15T14:49:22Z
updated_at: 2020-02-01T10:15:53Z
url: https://github.com/clap-rs/clap/issues/532
synced_at: 2026-01-10T01:26:30Z
---

# The load_yaml!() macro should not be relative to the working directory

---

_Issue opened by @Lindenk on 2016-06-15 14:49_

This seems like a new development, or maybe I just haven't noticed before, but it looks like `load_yaml!()` loads relative to the working directory rather than either the base of the project, or the `src` directory. This causes a problem when running `cargo build` from a directory that wasn't expected.


---

_Comment by @sru on 2016-06-16 03:18_

`load_yaml!` uses [`include_str!`](http://doc.rust-lang.org/stable/std/macro.include_str!.html) internally.

> The file is located relative to the current file (similarly to how modules are found),


---

_Label `T: RFC / question` added by @kbknapp on 2016-06-23 18:57_

---

_Label `W: wont do` added by @kbknapp on 2016-06-23 18:58_

---

_Comment by @kbknapp on 2016-06-23 19:05_

Like @sru said, this uses the `include_str!` internally so it's not something we could easily change, or do so without a runtime cost.

If you've got ideas on a new method to add which is relative to the project root I'm not against it. But would also like to make sure it's worth the effort.


---

_Closed by @kbknapp on 2016-06-23 19:05_

---

_Label `C: yaml parser` added by @pksunkara on 2020-02-01 10:15_

---
