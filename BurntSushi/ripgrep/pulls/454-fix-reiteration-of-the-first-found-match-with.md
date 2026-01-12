```yaml
number: 454
title: Fix reiteration of the first found match with --only-mathing flag
type: pull_request
state: merged
author: kpp
labels: []
assignees: []
merged: true
base: master
head: bug_451_only_mathing_digits
created_at: 2017-04-20T15:05:38Z
updated_at: 2017-04-23T12:21:48Z
url: https://github.com/BurntSushi/ripgrep/pull/454
synced_at: 2026-01-12T18:23:13Z
```

# Fix reiteration of the first found match with --only-mathing flag

---

_@kpp_

Fixes #451 

```
~/ripgrep$ cat tests/digits.txt 
1 2 3
123
~/ripgrep$ cargo run -- "\d" tests/digits.txt -o --column
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg '\d' tests/digits.txt -o --column`
1:1:1
1:3:2
1:5:3
2:1:1
2:2:2
2:3:3
~/ripgrep$ cargo run -- "\d+" tests/digits.txt -o --column
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg '\d+' tests/digits.txt -o --column`
1:1:1
1:3:2
1:5:3
2:1:123

```

---

_Review comment by @BurntSushi on `tests/tests.rs`:1618 on 2017-04-20 15:07_

Could you use a raw string literal for this? e.g., `r"\d"`.

---

_Review comment by @BurntSushi on `tests/tests.rs`:1620 on 2017-04-20 15:07_

I don't think you need this.

---

_Review comment by @BurntSushi on `tests/tests.rs`:1612 on 2017-04-20 15:08_

This test looks good. Could you add another test that approximately mimics the bug report in #451? e.g., Test that the actual results printed are correct instead of just column numbers.

---

_@BurntSushi requested changes on 2017-04-20 15:12_

Thanks for this fix! I think this mostly looks good, but I left some nits on the test. I also think we should include a regression test that roughly matches what the reporter initially presented.

---

_@kpp reviewed on 2017-04-20 15:14_

---

_Review comment by @kpp on `tests/tests.rs`:1618 on 2017-04-20 15:14_

Sure

---

_@kpp reviewed on 2017-04-20 15:18_

---

_Review comment by @kpp on `tests/tests.rs`:1620 on 2017-04-20 15:18_

You are right

---

_Review comment by @kpp on `tests/tests.rs`:1612 on 2017-04-20 15:29_

Ah. I see. Yep

---

_@kpp reviewed on 2017-04-20 15:29_

---

_Comment by @kpp on 2017-04-21 11:42_

@BurntSushi ?

---

_@BurntSushi approved on 2017-04-21 12:11_

---

_Merged by @BurntSushi on 2017-04-21 12:11_

---

_Closed by @BurntSushi on 2017-04-21 12:11_

---

_Comment by @BurntSushi on 2017-04-21 12:12_

Looks good, thank you!

---

_Branch deleted on 2017-04-23 12:21_

---
