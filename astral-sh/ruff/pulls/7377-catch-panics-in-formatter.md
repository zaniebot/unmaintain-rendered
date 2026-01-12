```yaml
number: 7377
title: Catch panics in formatter
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format-err
created_at: 2023-09-14T02:26:35Z
updated_at: 2023-09-14T15:44:37Z
url: https://github.com/astral-sh/ruff/pull/7377
synced_at: 2026-01-12T02:39:09Z
```

# Catch panics in formatter

---

_Pull request opened by @charliermarsh on 2023-09-14 02:26_

## Summary

This PR ensures that we catch and render panics in the formatter identically to other kinds of errors. It also improves the consistency in error rendering throughout and makes a few stylistic changes to the messages.

Closes https://github.com/astral-sh/ruff/issues/7247.

## Test Plan

I created a file `foo.py` with a syntax error, and a file `bar.py` with an intentional panic.

<img width="1624" alt="Screen Shot 2023-09-13 at 10 25 22 PM" src="https://github.com/astral-sh/ruff/assets/1309177/605c2839-ad02-4376-a2e9-d5a593ab660f">

<img width="1624" alt="Screen Shot 2023-09-13 at 10 25 24 PM" src="https://github.com/astral-sh/ruff/assets/1309177/b1381909-157c-48cb-9630-d0bbfcb1b640">


---

_Label `formatter` added by @charliermarsh on 2023-09-14 02:31_

---

_@charliermarsh reviewed on 2023-09-14 02:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/linter.rs`:556 on 2023-09-14 02:31_

This re-stylizes as "Ruff" instead of `ruff`.

---

_@charliermarsh reviewed on 2023-09-14 02:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/linter.rs`:545 on 2023-09-14 02:33_

This is consistent with our `error!` and `warn!` formatting (bold the colon).

---

_@charliermarsh reviewed on 2023-09-14 02:35_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:115 on 2023-09-14 02:35_

These don't show up if you run with `--quiet`, which I guess is correct?

---

_@charliermarsh reviewed on 2023-09-14 02:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 02:36_

This was causing tracing to print the error, but we want to handle reporting it downstream of this.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-14 02:36_

---

_Review requested from @konstin by @charliermarsh on 2023-09-14 02:36_

---

_Comment by @github-actions[bot] on 2023-09-14 02:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@dhruvmanila approved on 2023-09-14 03:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 06:39_

Shouldn't it only print the error when the tracing level is `err`? @konstin can we tune our tracing setup in the cli instead? I would want to keep errors showing, e.g. in the VS code output panel (once rewritten to rust)

---

_@MichaReiser approved on 2023-09-14 06:39_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format.rs`:115 on 2023-09-14 07:57_

I check that both cargo and black still print errors with `--quiet`. Should we do the same? I think of quiet as "don't show status messages", but i'm against swallowing errors

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 08:01_

how would you determine whether to show these errors as log messages, as opposed to other not separately handled `error!()` that we always want to show?

---

_@konstin approved on 2023-09-14 08:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 08:41_

I guess the main challenge is that we use `log` for both user-facing errors and tracing. I would recommend using the `log-tracing` for now. We never want to log any tracing events as user-visible messages. 

---

_@MichaReiser reviewed on 2023-09-14 08:41_

---

_@konstin reviewed on 2023-09-14 08:45_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 08:45_

switching to tracing is easy, i just have to figure out how to get the currrent `warning`/`error` formatting there

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 08:49_

I'm not sure if we are on the same page. What I'm trying to say is that I don't think we should use tracing to show user facing errors. Actually, we should avoid that tracing uses `log` to output its messages because they then end up in the console output. 

I think my preference would be to instead use e.g. a rotating file logger to which we write our tracing logs. This would give us an additional debugging tool as well and prevents that the two log-streams from interleaving. 

---

_@MichaReiser reviewed on 2023-09-14 08:49_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 10:44_

No the logging works, i meant something simpler: `fern` gives us custom formatting for warnings and errors, for tracing we need to either switch to tracing logging style or implement our own layer that formats them in the `**warning**: {message}` style:
```
$ ruff --select PLR2 --no-cache asdf
warning: Failed to lint asdf: No such file or directory (os error 2)
$ target/debug/ruff --select PLR2 --no-cache asdf
2023-09-14T10:39:55.819547Z  WARN ruff_cli::commands::check: Failed to lint asdf: No such file or directory (os error 2)  
```

![image](https://github.com/astral-sh/ruff/assets/6826232/e8ac3041-cef5-4511-bde9-34c022b536b1)

I surprisingly couldn't find any example of how to intersect `warn!` and `error!` nor how to get our plain warn/error style without reimplementing the whole event formatting.

---

_@konstin reviewed on 2023-09-14 10:44_

---

_@MichaReiser reviewed on 2023-09-14 11:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 11:05_

But should tracing warnings even be user facing? I see tracing as a development tool and not a way to communicate to users. 

---

_@charliermarsh reviewed on 2023-09-14 15:43_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:115 on 2023-09-14 15:43_

I'll consider this separately.

---

_Merged by @charliermarsh on 2023-09-14 15:44_

---

_Closed by @charliermarsh on 2023-09-14 15:44_

---

_Branch deleted on 2023-09-14 15:44_

---

_@charliermarsh reviewed on 2023-09-14 15:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/lib.rs`:123 on 2023-09-14 15:44_

(I interpreted this discussion as non-blocking for the PR, but let me know if that's incorrect.)

---
