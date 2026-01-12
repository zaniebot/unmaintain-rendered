```yaml
number: 3118
title: "fix: exit code for -q --files-without-match combination"
type: pull_request
state: closed
author: wislertt
labels: []
assignees: []
base: master
head: fix-exit-code
created_at: 2025-08-02T14:07:59Z
updated_at: 2025-08-20T01:00:40Z
url: https://github.com/BurntSushi/ripgrep/pull/3118
synced_at: 2026-01-12T18:23:15Z
```

# fix: exit code for -q --files-without-match combination

---

_@wislertt_

This is to resolve issue #3108 

Exit codes were inverted when using -q with --files-without-match. Quiet mode was overriding the search mode logic.

Before:
```
echo "haystack ......" | rg -q --files-without-match needle # returned 1 ❌
echo "haystack needle" | rg -q --files-without-match needle # returned 0 ❌
```

After:
```
echo "haystack ......" | rg -q --files-without-match needle # returns 0 ✅
echo "haystack needle" | rg -q --files-without-match needle # returns 1 ✅
```

Fix: Don't let quiet mode override FilesWithoutMatch summary kind.

One line change in hiargs.rs printer logic.

---

_Comment by @BurntSushi on 2025-08-20 00:56_

This change is bunk unfortunately. ripgrep's test suite doesn't check that `-q --files-without-match` emits nothing on stdout, but this patch makes it... not be quiet any more.

The problem is that the summary printer doesn't distinguish "quiet" mode with "quiet inverted match" mode. This is an API design bug in ripgrep's internal libraries. Sigh.

Nice find though.

---

_Comment by @BurntSushi on 2025-08-20 00:56_

Specifically, with this PR, this test fails:

```
// See: https://github.com/BurntSushi/ripgrep/issues/3108
rgtest!(r3108_files_without_match_quiet_exit, |dir: Dir, _: TestCommand| {
    dir.create("yes-match", "abc");
    dir.create("non-match", "xyz");

    dir.command().args(&["-q", "abc", "non-match"]).assert_exit_code(1);
    dir.command().args(&["-q", "abc", "yes-match"]).assert_exit_code(0);
    dir.command()
        .args(&["--files-with-matches", "-q", "abc", "non-match"])
        .assert_exit_code(1);
    dir.command()
        .args(&["--files-with-matches", "-q", "abc", "yes-match"])
        .assert_exit_code(0);

    dir.command()
        .args(&["--files-without-match", "abc", "non-match"])
        .assert_exit_code(0);
    dir.command()
        .args(&["--files-without-match", "abc", "yes-match"])
        .assert_exit_code(1);

    let got = dir
        .command()
        .args(&["--files-without-match", "abc", "non-match"])
        .stdout();
    eqnice!("non-match\n", got);

    dir.command()
        .args(&["--files-without-match", "-q", "abc", "non-match"])
        .assert_exit_code(0);
    dir.command()
        .args(&["--files-without-match", "-q", "abc", "yes-match"])
        .assert_exit_code(1);

    let got = dir
        .command()
        .args(&["--files-without-match", "-q", "abc", "non-match"])
        .stdout();
    eqnice!("\n", got);
});
```

with:

```
running 1 test
test regression::r3108_files_without_match_quiet_exit ... FAILED

failures:

---- regression::r3108_files_without_match_quiet_exit stdout ----

thread 'regression::r3108_files_without_match_quiet_exit' (1875804) panicked at tests/regression.rs:1530:5:

printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
non-match

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

---

_Closed by @BurntSushi on 2025-08-20 00:56_

---

_Comment by @BurntSushi on 2025-08-20 01:00_

This part in the code is the problem:

https://github.com/BurntSushi/ripgrep.git/blob/119a58a400ea948c2d2b0cd4ec58361e74478641/crates/printer/src/summary.rs#L506-L511

For `SummaryKind::PathWithoutMatch`, the case of whether a match has occurred or not is correctly inverted. But for `SummaryKind::Quiet`, the printer doesn't know we want to invert the match.

---
