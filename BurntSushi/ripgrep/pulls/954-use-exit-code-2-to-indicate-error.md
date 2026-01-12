```yaml
number: 954
title: Use exit code 2 to indicate error
type: pull_request
state: merged
author: sirreal
labels: []
assignees: []
merged: true
base: master
head: update/exit-code-2-on-error
created_at: 2018-06-18T07:34:30Z
updated_at: 2018-06-19T12:54:14Z
url: https://github.com/BurntSushi/ripgrep/pull/954
synced_at: 2026-01-12T18:23:13Z
```

# Use exit code 2 to indicate error

---

_@sirreal_

Exit code `1` was shared to indicate both "no results" and "error." Use status code `2` to indicate errors, similar to grep's behavior.

Fixes #948

Adds integration tests for exit codes.
Adds test helper `WorkDir::assert_exit_code(&self, expected_code: i32, cmd: &mut process::Command)` to match arbitrary exit codes in tests.


## Testing

```sh
cargo test exit_code
```

## Manual testing

Verify that the exit code is now `2` for error:

```
$ cargo run -- '*' src/main.rs ; echo "Status code: $?"

    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg '*' src/main.rs`
regex parse error:
    *
    ^
error: repetition operator missing expression
Status code: 2
```

And remains `0` when matches are found:

```
$ cargo run -- 'main' src/main.rs ; echo "Status code: $?"

    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg main src/main.rs`
56:fn main() {
Status code: 0
```

And `1` for no matches:

```
cargo run -- 'this_does_not_match' src/main.rs ; echo "Status code: $?"


    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg this_does_not_match src/main.rs`
Status code: 1
```

---

_Comment by @BurntSushi on 2018-06-18 12:51_

@sirreal What do you think about adding few tests for this? We should be able to capture your manual tests in [ripgrep's integration tests](https://github.com/BurntSushi/ripgrep/blob/master/tests/tests.rs).

The test helpers themselves might need some additions to test this. For example, you might consider adding something similar to [`assert_err`](https://github.com/BurntSushi/ripgrep/blob/223d7d9846bff4a9aaf6ba84f5662a1ee7ffa900/tests/workdir.rs#L259-L274) that lets one specifically probe the exit status code. In that code, `o.status` is a `std::process::ExitStatus`, which means it should be easy to [retrieve the exit code](https://doc.rust-lang.org/std/process/struct.ExitStatus.html#method.code).

---

_Comment by @sirreal on 2018-06-18 14:16_

I was wondering about testing. I'm happy to look into that, I'll investigate soon 👍 

---

_Review comment by @sirreal on `tests/tests.rs`:2198 on 2018-06-18 19:15_

Should match any non-empty file

---

_Review comment by @sirreal on `tests/tests.rs`:2208 on 2018-06-18 19:16_

Highly unlikely to match any file. 10 random hex digits repeated 10 times.

---

_Review comment by @sirreal on `tests/tests.rs`:2218 on 2018-06-18 19:16_

Invalid regex.

---

_@sirreal reviewed on 2018-06-18 19:17_

---

_Comment by @sirreal on 2018-06-18 19:17_

I've added some integration tests as suggested, I'd appreciate feedback.

---

_Review comment by @sirreal on `tests/workdir.rs`:279 on 2018-06-18 20:04_

These `expect`s can just become `unwrap`.

---

_@sirreal reviewed on 2018-06-18 20:04_

---

_@BurntSushi approved on 2018-06-19 11:40_

Excellent! This is perfect as-is, thank you so much. :-)

---

_Merged by @BurntSushi on 2018-06-19 11:41_

---

_Closed by @BurntSushi on 2018-06-19 11:41_

---

_Branch deleted on 2018-06-19 12:38_

---

_Comment by @sirreal on 2018-06-19 12:54_

A pleasure! 🙌 

---
