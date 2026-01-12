```yaml
number: 1543
title: "Arguments with takes_value(false) still accept \"compact\"-style arguments"
type: issue
state: closed
author: sfackler
labels:
  - C-bug
assignees: []
created_at: 2019-09-04T20:57:00Z
updated_at: 2022-03-21T16:34:51Z
url: https://github.com/clap-rs/clap/issues/1543
synced_at: 2026-01-12T16:14:11Z
```

# Arguments with takes_value(false) still accept "compact"-style arguments

---

_@sfackler_

# Rust Version

* 1.37.0

### Affected Version of clap

* 2.33.0

### Bug or Feature Request Summary

Arguments configured to not take a value still successfully parse from the `--key=value` form.

### Expected Behavior Summary

The command line should fail to validate (or only accept `--key=true` or `--key=false`.

### Actual Behavior Summary

The command line successfully validates as if it just contained `--key`.

### Sample Code or Link to Sample Code

```rust
use clap::*;

fn main() {
    let matches = App::new("foo")
        .arg(Arg::with_name("foobar").long("foobar").takes_value(false).multiple(false))
        .get_matches_from(["foo", "--foobar=false"].iter());
    println!("{:?}", matches)
}
```
https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=d08679674fa982b892dcc26d8ed8bb25

---

_Label `C: options` added by @CreepySkeleton on 2020-02-01 12:25_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-01 12:25_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-01 12:25_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 12:25_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-01 12:25_

---

_Removed from milestone `3.0` by @pksunkara on 2020-04-09 08:10_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:10_

---

_Referenced in [sharkdp/hexyl#109](../../sharkdp/hexyl/issues/109.md) on 2020-10-24 18:53_

---

_Label `W: 2.x` removed by @pksunkara on 2020-10-26 07:58_

---

_Referenced in [sharkdp/fd#727](../../sharkdp/fd/issues/727.md) on 2021-02-13 21:12_

---

_Referenced in [clap-rs/clap#2484](../../clap-rs/clap/issues/2484.md) on 2021-05-18 01:33_

---

_Referenced in [clap-rs/clap#2646](../../clap-rs/clap/pulls/2646.md) on 2021-07-30 19:23_

---

_Referenced in [clap-rs/clap#2619](../../clap-rs/clap/pulls/2619.md) on 2021-07-30 20:08_

---

_Closed by @pksunkara on 2021-08-01 11:31_

---

_Comment by @haxpor on 2022-03-21 11:18_

Can we reopen this issue? I tested this from the tip of the git tree and the problem is still happening, also I faced the same issue on my own code using `3.1.6`.

Modify `tests/builder/multiple_occurrences.rs` (just for the sake of quick dirty test)

```rust
#[cfg(feature = "env")]
#[test]
fn multiple_occurrences_of_before_env() {
    let cmd = Command::new("mo_before_env").arg(
        Arg::new("verbose")
            .env("VERBOSE")
            .short('v')
            .long("verbose")
            .takes_value(false)
            .multiple_occurrences(true),
    );

    let m = cmd.clone().try_get_matches_from(vec![""]);
    assert!(m.is_ok(), "{}", m.unwrap_err());
    assert_eq!(m.unwrap().occurrences_of("verbose"), 0);

    let m = cmd.clone().try_get_matches_from(vec!["", "-v"]);
    assert!(m.is_ok(), "{}", m.unwrap_err());
    assert_eq!(m.unwrap().occurrences_of("verbose"), 1);

    let m = cmd.clone().try_get_matches_from(vec!["", "-vv"]);
    assert!(m.is_ok(), "{}", m.unwrap_err());
    assert_eq!(m.unwrap().occurrences_of("verbose"), 2);
    let m = cmd.clone().try_get_matches_from(vec!["", "-vvv"]);
    assert!(m.is_ok(), "{}", m.unwrap_err());
    assert_eq!(m.unwrap().occurrences_of("verbose"), 3);

    // this chunk of code is added to test
    // notice `-vvv=false`, test result is fine, no error out
    let m = cmd.clone().try_get_matches_from(vec!["", "-vvv=false"]);
    assert!(m.is_ok(), "{}", m.unwrap_err());
    assert_eq!(m.unwrap().occurrences_of("verbose"), 3);
}
```

as well I didn't see relevant changes that fix this issue in #2619.
My apology if I'm wrong nor missing something.


---

_Comment by @epage on 2022-03-21 16:34_

I just tried reproducing with the code sample you gave and it failed
```
thread 'multiple_occurrences::multiple_occurrences_of_before_env' panicked at 'error: Found argument '-=' which wasn't expected, or isn't valid in this context

        If you tried to supply `-=` as a value rather than a flag, use `-- -=`

USAGE:
    mo_before_env [OPTIONS]

For more information try --help
', tests/builder/multiple_occurrences.rs:128:5
```

---

_Referenced in [solana-labs/solana-program-library#4511](../../solana-labs/solana-program-library/issues/4511.md) on 2023-06-08 23:26_

---
