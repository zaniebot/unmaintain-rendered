```yaml
number: 3727
title: "Relax version of `os_str_bytes`"
type: issue
state: closed
author: jerome-pouiller
labels: []
assignees: []
created_at: 2022-05-13T07:16:34Z
updated_at: 2022-05-13T11:59:05Z
url: https://github.com/clap-rs/clap/issues/3727
synced_at: 2026-01-12T16:14:15Z
```

# Relax version of `os_str_bytes`

---

_@jerome-pouiller_

`os_str_bytes` 6.0 now depends on Rust 1.52.

`clap_lex` depends on `os_str_bytes` 6.0 but, it seems that version 4.0 is sufficient.

It matters because the version of `rustc` provided by the last Debian (and the last RaspbianOS) is 1.48.0. Do you think you could relax the dependency of `os_str_bytes` in '>= 4.0, < 7.0' ?


---

_Comment by @epage on 2022-05-13 11:59_

Our official [MSRV policy is "two versions back"](https://github.com/clap-rs/clap#aspirations) though we've been letting it go a little (1) as [an experimental policy of "6 months back"](https://github.com/clap-rs/clap/issues/3267) and (2) out of laziness.  There are newer features we want to be using that I do not see us holding back for once we can.

Even if we loosened the version requirement for `os_str_bytes`, clap itself is using features from 1.52.  See https://github.com/clap-rs/clap/issues/3267 for more of a discussion on this.

---

_Closed by @epage on 2022-05-13 11:59_

---

_Referenced in [rust-lang/cargo#9930](../../rust-lang/cargo/issues/9930.md) on 2023-03-30 12:16_

---
