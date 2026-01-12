```yaml
number: 2613
title: "Unsetting `AllowInvalidUtf8` does nothing"
type: issue
state: closed
author: epage
labels:
  - C-bug
assignees: []
created_at: 2021-07-21T20:59:16Z
updated_at: 2021-08-18T21:50:43Z
url: https://github.com/clap-rs/clap/issues/2613
synced_at: 2026-01-12T16:14:13Z
```

# Unsetting `AllowInvalidUtf8` does nothing

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.53.0 (53cb7b09b 2021-06-17)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```
app.unset_settings(AppSettings::AllowInvalidUtf8)
```

### Steps to reproduce the bug with the above code

Pass in invalid UTF-8

### Actual Behaviour

Its allowed

### Expected Behaviour

Its rejected

### Additional Context

I actually found this by inspection.
```bash
$ rg AllowInvalidUtf8
tests/app_settings.rs
598:    assert!(&m.is_set(AppSettings::AllowInvalidUtf8));
602:        .unset_global_setting(AppSettings::AllowInvalidUtf8)
604:    assert!(!m.is_set(AppSettings::AllowInvalidUtf8), "{:#?}", m);

src/build/app/settings.rs
83:    AllowInvalidUtf8("allowinvalidutf8")
197:    ///   //.setting(AppSettings::AllowInvalidUtf8)
215:    AllowInvalidUtf8,
1151:            AppSettings::AllowInvalidUtf8
```

And in `AppSettings`
```rust
    // TODO: Either this or StrictUtf8
    AllowInvalidUtf8,
```

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-07-21 20:59_

---

_Comment by @hosseind88 on 2021-07-22 16:45_

can anyone explain to me please what should I do to fix this issue?
I checked code but I don't understand what we should change about TODO

---

_Comment by @epage on 2021-07-22 17:10_

I believe `AllowInvalidUtf8` was being renamed to `StrictUtf8` but the old name was left in.  The name should probably be removed and people use `StrictUtf8`.

This needs to get added to the changelog to help people when upgrading from clap2 to clap3

---

_Referenced in [clap-rs/clap#2623](../../clap-rs/clap/pulls/2623.md) on 2021-07-26 13:39_

---

_Comment by @hosseind88 on 2021-07-26 13:41_

thanks, I have submitted the PR

---

_Added to milestone `3.0` by @pksunkara on 2021-08-02 08:54_

---

_Label `C: settings` added by @pksunkara on 2021-08-09 02:12_

---

_Closed by @pksunkara on 2021-08-18 21:50_

---
