```yaml
number: 1740
title: "ignore: Provide IO Error"
type: pull_request
state: merged
author: epage
labels: []
assignees: []
merged: true
base: master
head: ignore-io
created_at: 2020-11-22T03:38:12Z
updated_at: 2020-11-23T15:26:02Z
url: https://github.com/BurntSushi/ripgrep/pull/1740
synced_at: 2026-01-12T18:23:14Z
```

# ignore: Provide IO Error

---

_@epage_

`ignore::Error` wraps `std::io::Error` with additional information (as
well as expose non-IO errors).  For people wanting to inspect what the
error is, they have to recursively match the Enum.  Like with `is_io()`,
this provides `io_error` and `into_io_error` helpers to do this for
the user.  These are written to be consistent with `walkdir::Error`.

---

_Review comment by @BurntSushi on `crates/ignore/src/lib.rs`:262 on 2020-11-22 15:10_

I think it would probably be better to just expose a `&std::io::Error`, no? It gives strictly more information and also provides access to a `std::io::ErrorKind`. Might as well provide an `into_io_error` as well.

This is how walkdir does it: https://docs.rs/walkdir/2.3.1/walkdir/struct.Error.html#method.io_error

---

_@BurntSushi requested changes on 2020-11-22 15:10_

---

_Renamed from "ignore: Provide IO ErrorKind" to "ignore: Provide IO Error" by @epage on 2020-11-23 14:52_

---

_@epage reviewed on 2020-11-23 15:01_

---

_Review comment by @epage on `crates/ignore/src/lib.rs`:262 on 2020-11-23 15:01_

Good to know there is a baseline to be consistent with.  I copied things over from there.

When writing this, I was split on which direction to go.  I saw the `ErrorKind` being useful for programmatic use and `Error` being useful for error propagation but wasn't as clear what the value of a `&Error` would be.  However, there is value in not being prescriptive and letting the user decide what has value.  And then comes along consistency :)

---

_Comment by @epage on 2020-11-23 15:02_

Random asides from before I saw `WalkDir::Error`:
- I considered naming `io_error` as `io_error_ref` though from the API guidelines `as_io_error_ref` would probably be the most "correct" but at that point its a bit verbose.
- I considered `into_io_error` returning a `Result<std::io::Error, Self>`, kind of like what some String operations do, to avoid any data loss but the `Partial` case made this messier.  I figured someone could take the non-IO case and `to_string` it and put it into an `std::io::Error`.

---

_@BurntSushi approved on 2020-11-23 15:17_

Nice, LGTM!

---

_Merged by @BurntSushi on 2020-11-23 15:19_

---

_Closed by @BurntSushi on 2020-11-23 15:19_

---

_Branch deleted on 2020-11-23 15:25_

---

_Comment by @BurntSushi on 2020-11-23 15:26_

This PR is on crates.io in `ignore 0.4.17`.

---
