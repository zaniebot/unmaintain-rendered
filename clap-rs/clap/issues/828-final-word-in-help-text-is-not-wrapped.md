---
number: 828
title: Final word in help text is not wrapped
type: issue
state: closed
author: mgeisler
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2017-01-29T20:00:08Z
updated_at: 2018-08-02T03:30:00Z
url: https://github.com/clap-rs/clap/issues/828
synced_at: 2026-01-10T01:26:36Z
---

# Final word in help text is not wrapped

---

_Issue opened by @mgeisler on 2017-01-29 20:00_

### Affected Version of clap

Tested with latest `master`, d29d7cc.

### Expected Behavior Summary

The final word in the help text should be wrapped.

### Actual Behavior Summary

The final word remain with the next to final word, that is, the last space is ignored.

### Steps to Reproduce the issue

```
#[test]
fn final_word_wrapping() {
    let app = App::new("ctest").version("0.1").set_term_width(24);
    assert!(test::compare_output(app, "ctest --help", FINAL_WORD_WRAPPING, false));
}

static FINAL_WORD_WRAPPING: &'static str = "ctest 0.1

USAGE:
    ctest

FLAGS:
    -h, --help
            Prints help
            information
    -V, --version
            Prints
            version information
";
```

The `"version information"` line is longer than 12 characters, despite `information` being only 11 characters wide.

I have a fix for this that I'll post next.

---

_Comment by @kbknapp on 2017-01-30 02:33_

Interesting, I wonder if it's counting the space? Could you post a gist of the output when you compile clap with `features = ["debug"]` and run the test program?

---

_Label `C: help message` added by @kbknapp on 2017-01-30 02:33_

---

_Label `D: intermediate` added by @kbknapp on 2017-01-30 02:33_

---

_Label `P3: want to have` added by @kbknapp on 2017-01-30 02:33_

---

_Label `T: bug` added by @kbknapp on 2017-01-30 02:33_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-30 02:33_

---

_Added to milestone `2.20.2` by @kbknapp on 2017-01-30 04:36_

---

_Referenced in [clap-rs/clap#832](../../clap-rs/clap/pulls/832.md) on 2017-01-30 06:53_

---

_Comment by @mgeisler on 2017-01-30 06:55_

I've put up a PR for fixing this and a small related bug. As explained, I hope to do a drop-in replacement later with my [textwrap crate][1], but wanted to make sure the output didn't change first. So I compared the existing code with the wrapping done by that crate and found small problems.

[1]: https://crates.io/crates/textwrap

---

_Closed by @homu on 2017-01-31 01:24_

---
