```yaml
number: 1199
title: crlf tests fail on Fedora
type: issue
state: closed
author: igor-raits
labels:
  - bug
assignees: []
created_at: 2019-02-12T14:33:01Z
updated_at: 2019-02-16T14:40:26Z
url: https://github.com/BurntSushi/ripgrep/issues/1199
synced_at: 2026-01-12T16:13:23Z
```

# crlf tests fail on Fedora

---

_@igor-raits_

```
---- feature::f416_crlf_only_matching stdout ----
thread 'feature::f416_crlf_only_matching' panicked at '
printed outputs differ!
expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Sherlock
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Sherlock

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/feature.rs:455:5
note: Run with `RUST_BACKTRACE=1` for a backtrace.
---- json::crlf stdout ----
thread 'json::crlf' panicked at 'assertion failed: `(left == right)`
  left: `SubMatch { m: Text { text: "Sherlock\r" }, start: 56, end: 65 }`,
 right: `SubMatch { m: Text { text: "Sherlock" }, start: 56, end: 64 }`', tests/json.rs:298:5
failures:
    feature::f416_crlf_only_matching
    json::crlf
```

---

_Comment by @BurntSushi on 2019-02-12 14:42_

The Debian folks ran into the same test failures. The issue is that a new version of the `grep-searcher` crate was released, which includes a CRLF related fix. But in order for it to work correctly with ripgrep on master, it also needs a corresponding fix in `grep-regex`, which I haven't released yet.

With that said, ripgrep 0.10.0, which is the latest release, should be working fine. Are you trying to use ripgrep master with only the latest releases of crates? I'm not sure that's always guaranteed to work.

---

_Label `question` added by @BurntSushi on 2019-02-12 14:42_

---

_Comment by @igor-raits on 2019-02-12 15:56_

> With that said, ripgrep 0.10.0, which is the latest release, should be working fine. Are you trying to use ripgrep master with only the latest releases of crates? I'm not sure that's always guaranteed to work.

In Fedora we always use latest crates. Which means that it picked up latest grep-searcher.

---

_Comment by @BurntSushi on 2019-02-12 15:59_

@ignatenkobrain I _think_ I can release a new version of `grep-regex` and it might fix it this time, but I don't know if I can provide that guarantee in all cases. Otherwise, it requires the pace of ripgrep releases to be on par with the pace of crate releases, and I'm just never going to do that.

---

_Comment by @igor-raits on 2019-02-12 16:00_

@BurntSushi but doesn't that mean when you changed handling of crlf in grep-searcher it should have been a breaking change? Since that changes semantics.

---

_Comment by @BurntSushi on 2019-02-12 16:56_

No. It's a bug fix. It changes errant behavior, not semantics.

---

_Comment by @igor-raits on 2019-02-12 17:18_

Then I guess the best way forward would be to publish new version of grep-regex.

---

_Comment by @BurntSushi on 2019-02-16 14:40_

`grep-regex 0.1.2` is now on crates.io.

---

_Closed by @BurntSushi on 2019-02-16 14:40_

---

_Label `bug` added by @BurntSushi on 2019-02-16 14:40_

---

_Label `question` removed by @BurntSushi on 2019-02-16 14:40_

---
