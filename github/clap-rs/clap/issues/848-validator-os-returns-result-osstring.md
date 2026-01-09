---
number: 848
title: "validator_os returns Result<(), OsString>"
type: issue
state: closed
author: ssokolow
labels:
  - A-validators
assignees: []
created_at: 2017-02-11T23:39:13Z
updated_at: 2018-08-02T03:30:01Z
url: https://github.com/clap-rs/clap/issues/848
synced_at: 2026-01-07T13:12:19-06:00
---

# validator_os returns Result<(), OsString>

---

_Issue opened by @ssokolow on 2017-02-11 23:39_

### Affected Version of clap
2.20.3 and anything API-compatible with it.

### Expected Behavior Summary

Callbacks for `validator_os` should return `Result<T, String>` for some `T` because:

* Error messages are intended for display
* It's consistent with callbacks for `validator` in a context where the usage doesn't differ
* It's easy to incorporate `OsStr`/`OsString` into `format!` output via the `Debug` trait.
* There's no facility for formatting natively with `OsStr`/`OsString`
* There's apparently no good reason to use `Result<T, OsString>`

### Actual Behavior Summary

* I have to stick `.into()` onto the end of every `Err` case that I return from a path-handling validator.
* I get questions and then "eww" reactions when people not familiar with clap encounter `Result<(), OsString>`.

### Sample Code or Link to Sample Code

```rust
pub fn filename_valid(value: &OsStr) -> Result<(), OsString> {
    // TODO: Switch to using to_bytes() once it's stabilized
    if value.to_string_lossy().chars().any(|c| is_bad_for_fname(&c)) {
        Err(format!("Name contains invalid characters: {:?}", value).into())
    } else {
        Ok(())
    }
}
```

I think this should be reconsidered for clap 3.x

---

_Comment by @kbknapp on 2017-02-12 00:03_

Hmm, I see your point about errors being designed for display anyways. I'm inclined to agree it should be changed to `Result<(), String>` in 3.x. I'd be curious if anyone has a use case where returning a String wouldn't be acceptable?

I'll tentatively mark this for change in 3.x, but am open to discussion.

For now, `.into()` will have to do 😞 

---

_Label `C: validators` added by @kbknapp on 2017-02-12 00:03_

---

_Label `D: easy` added by @kbknapp on 2017-02-12 00:03_

---

_Label `P4: nice to have` added by @kbknapp on 2017-02-12 00:03_

---

_Label `T: RFC / question` added by @kbknapp on 2017-02-12 00:03_

---

_Label `W: 3.x` added by @kbknapp on 2017-02-12 00:03_

---

_Added to milestone `3.0` by @kbknapp on 2017-02-12 00:03_

---

_Referenced in [clap-rs/clap#981](../../clap-rs/clap/pulls/981.md) on 2017-06-08 17:48_

---

_Removed from milestone `3.0` by @kbknapp on 2018-02-02 01:57_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:57_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-05 15:19_

---

_Referenced in [clap-rs/clap#1165](../../clap-rs/clap/issues/1165.md) on 2018-02-05 15:23_

---

_Referenced in [rust-cli/team#26](../../rust-cli/team/issues/26.md) on 2018-03-25 00:12_

---

_Comment by @kbknapp on 2018-07-22 01:07_

This has been implemented on v3-master

---

_Closed by @kbknapp on 2018-07-22 01:07_

---
