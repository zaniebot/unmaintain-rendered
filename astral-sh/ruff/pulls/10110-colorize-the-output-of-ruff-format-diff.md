```yaml
number: 10110
title: Colorize the output of ruff format --diff
type: pull_request
state: merged
author: senadev42
labels:
  - cli
assignees: []
merged: true
base: main
head: colorize-format-diff
created_at: 2024-02-24T13:18:14Z
updated_at: 2024-03-01T12:34:44Z
url: https://github.com/astral-sh/ruff/pull/10110
synced_at: 2026-01-10T22:47:01Z
```

# Colorize the output of ruff format --diff

---

_Pull request opened by @senadev42 on 2024-02-24 13:18_

## Summary

Adds color to the output of ```ruff format --diff```. Implemented by extracting the logic inside of towriter() and using the Colored library.

## Test Plan

<!-- How was it tested? -->

Tested manually in development with one of the files until colored output was achieved.

```cargo run -p ruff -- format --diff crates/ruff_linter/resources/test/fixtures/pycodestyle/W29.py --no-cache```

Doesn't pass the ``cargo test``` format diff tests and I'm not sure if it's because something's wrong with the implementation or if the tests need to be updated. Would like some help in that direction either way.

Related issue #9984 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/source_kind.rs`:106 on 2024-02-24 13:50_

We should re-use your new diffing for the jupyter notebook case below. But that's something that you might have planned anyway

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/source_kind.rs`:125 on 2024-02-24 14:06_

Nit: We can iterate over the changes in the `hunk` and use the hunk's display to avoid matching on the first character. 
```rust
								// Individual hunks (section of changes)
                for hunk in unified_diff.iter_hunks() {
                    writeln!(writer, "{}", hunk.header())?;

                    // individual lines
                    for change in hunk.iter_changes() {
                        match change.tag() {
                            ChangeTag::Equal => write!(writer, " {}", change.value())?,
                            ChangeTag::Delete => write!(writer, "{}{}", "-".red(), change.value())?,
                            ChangeTag::Insert => {
                                write!(writer, "{}{}", "+".green(), change.value())?
                            }
                        }
                    }
                }
```


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/source_kind.rs`:108 on 2024-02-24 14:24_

This is not related to your change and might be worth a separate PR but showing the same header for both add and remove doesn't feel that useful... We might want to change this that the caller needs to provide labels.

---

_@MichaReiser reviewed on 2024-02-24 14:24_

I believe the reason why the tests fail is because the header ordering is now different:

* before: `---` before `+++`
* now: `+++` before `---`

I believe the other issue could be that you aren't writing the hunk header (the `@@`)


---

_Comment by @MichaReiser on 2024-02-24 14:31_

And yes, reading test failures that are diffs of diffs is extremely difficult. I had to play with the code myself to figure out what's wrong 

---

_@senadev42 reviewed on 2024-02-25 08:12_

---

_Review comment by @senadev42 on `crates/ruff_linter/src/source_kind.rs`:125 on 2024-02-25 08:12_

Oh this only colors the + and -  compared to also doing `change.value().green()` and `change.value().red()` for the Delete and Insert respectively, or is only coloring the symbols the intended behavior?



---

_Comment by @senadev42 on 2024-02-25 08:18_

> I believe the reason why the tests fail is because the header ordering is now different:
> 
> * before: `---` before `+++`
> * now: `+++` before `---`
> 
> I believe the other issue could be that you aren't writing the hunk header (the `@@`)

Yeah the headers were flipped compared to how the built in writer was doing it. Changed that and it passes the original tests it was failing and now it's just failing these two integration tests

```
test diff_shows_safe_fixes_by_default ... FAILED
test diff_shows_unsafe_fixes_with_opt_in ... FAILED
```

diff_shows_safe_fixes_by_default:
────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────
    0     0 │ success: false␊
    1     1 │ exit_code: 1␊
    2     2 │ ----- stdout -----␊
          3 │+--- ␊ < is green
          4 │++++ ␊ < is green
    3     5 │ @@ -0,0 +1 @@␊
    4     6 │ +# fix from stable-test-rule-safe-fix␊
    5     7 │ ␊
    6     8 │ ␊
    7     9 │ ----- stderr -----␊
    8       │-Would fix 1 error (1 additional fix available with `--unsafe-fixes`). < is red
         10 │+Would fix 1 error (1 additional fix available with `--unsafe-fixes`).␊ < is green
────────────┴───────────────────────────────────────────────

I think it has something to do with the ␊ which means linebreak stuff going on so I'll try to play around with the different types of write, writelin and '\n' (which is how the unextracted writer did it but it didn't pass linting) combinations and try to figure it out.

---

_Comment by @MichaReiser on 2024-02-26 09:10_

Interesting about the color change of the summary. That's unexpected. Maybe try again after https://github.com/astral-sh/ruff/pull/10127 merges. It mentions a fix related to bogus newlines

---

_Comment by @MichaReiser on 2024-03-01 08:51_

I didn't see any newline issues after upgrading to the latest insta version. So that seems fixed. I refactored the diff code a bit to avoid passing around `writers` and tested the output

![image](https://github.com/astral-sh/ruff/assets/1203881/6d694c80-ffbf-4143-ad7f-9136c4163be2)

I think this is good for merging. Thanks for working on this

---

_@MichaReiser approved on 2024-03-01 08:51_

---

_Marked ready for review by @MichaReiser on 2024-03-01 08:52_

---

_Merged by @MichaReiser on 2024-03-01 08:55_

---

_Closed by @MichaReiser on 2024-03-01 08:55_

---

_Comment by @github-actions[bot] on 2024-03-01 09:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `cli` added by @MichaReiser on 2024-03-01 09:04_

---

_Comment by @senadev42 on 2024-03-01 10:47_

Oh yeah it looks much cleaner now, won't lie was worried about how I was going to do that since I knew enough to know it was messy but not enough to know on how to not make it so. 

Also, any reason not to color the whole line (symbols + change.value()) like in the header instead of just the symbols?

(line 238, 239)

```rust
      match change.tag() {
          ChangeTag::Equal => write!(f, " {}", change.value())?,
          ChangeTag::Delete => write!(f, "{}{}", "-".red(), change.value())?,
            //                                                            ^.red()
          ChangeTag::Insert => write!(f, "{}{}", "+".green(), change.value())?,
             //                                                             ^.green()  
      }
```


---

_Comment by @MichaReiser on 2024-03-01 11:36_

> Also, any reason not to color the whole line (symbols + change.value()) like in the header instead of just the symbols?

Uhhh no, That's not intentional. Would you be interesting in doing a follow up PR?

---

_Comment by @senadev42 on 2024-03-01 12:34_

Follow up at #10183 

---
