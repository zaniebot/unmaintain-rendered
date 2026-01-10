---
number: 2242
title: "Panic: found positional argument which is not required with a lower index than a required positional argument"
type: issue
state: closed
author: nagisa
labels:
  - C-bug
assignees: []
created_at: 2020-12-07T16:03:36Z
updated_at: 2020-12-08T15:43:11Z
url: https://github.com/clap-rs/clap/issues/2242
synced_at: 2026-01-10T01:27:14Z
---

# Panic: found positional argument which is not required with a lower index than a required positional argument

---

_Issue opened by @nagisa on 2020-12-07 16:03_

### Make sure you completed the following tasks

- [ ] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

```rust
    let matches = clap::App::new(clap::crate_name!())
        .version(clap::crate_version!())
        .author(clap::crate_authors!())
        .about(clap::crate_description!())
        .arg(
            clap::Arg::with_name("dummy")
                .possible_value("dummy")
                .required(false)
                .hidden(true)
        )
        .arg(
            clap::Arg::with_name("BANANA")
                .required(true)
                .env("BANANA")
        )
        .arg(
            clap::Arg::with_name("PEACH")
                .required(true)
                .env("PEACH")
        ).get_matches();
```

### Steps to reproduce the issue

1. Run snippet above

### Version

* **Clap**: 2.33.3

### Actual Behavior Summary

This snippet when run panics.

### Expected Behavior Summary

There's no obvious reason it should panic.

### Debug output

```
DEBUG:clap:Parser::propagate_settings: self=cargo-phabricator, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
thread 'main' panicked at 'Found positional argument which is not required with a lower index than a required positional argument: "dummy" index 1', /home/nagisa/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/app/parser.rs:643:21
```

---

_Label `T: bug` added by @nagisa on 2020-12-07 16:03_

---

_Comment by @pksunkara on 2020-12-07 16:45_

There is clear reason showing why it's panicking. You haven't searched previous issues and discussions where this has been elaborated.

---

_Closed by @pksunkara on 2020-12-07 16:45_

---

_Comment by @nagisa on 2020-12-07 17:03_

> You haven't searched previous issues and discussions where this has been elaborated.

Either [github search is broken](https://github.com/clap-rs/clap/issues?q=is%3Aissue+You+haven%27t+searched+previous+issues+and+discussions+where+this+has+been+elaborated.) or there were no previous issues with this panic message. I did explicitly mark the checkbox that I did spend at least some effort looking through the closed issues.

---

_Comment by @pksunkara on 2020-12-07 17:10_

https://github.com/clap-rs/clap/discussions/2221

---

_Reopened by @pksunkara on 2020-12-08 15:43_

---

_Closed by @pksunkara on 2020-12-08 15:43_

---
