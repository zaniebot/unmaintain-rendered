```yaml
number: 9448
title: Add a idempotent fuzz_target for ruff_python_formatter
type: pull_request
state: merged
author: manunio
labels: []
assignees: []
merged: true
base: main
head: ruff-format-fuzz-target
created_at: 2024-01-09T11:34:37Z
updated_at: 2024-01-11T17:53:40Z
url: https://github.com/astral-sh/ruff/pull/9448
synced_at: 2026-01-10T22:57:09Z
```

# Add a idempotent fuzz_target for ruff_python_formatter

---

_Pull request opened by @manunio on 2024-01-09 11:34_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
This pull request attempts to add:
 - An idempotent harness for ruff_python_formatter.
 - A validity harness for ruff_python_formatter by (@addisoncrump).
 
Relevant issue: #9169.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Not needed.

---

_Comment by @addisoncrump on 2024-01-09 14:25_

Actually, this is valid syntax. You found precisely the kind of issue that the fuzzer should be looking for! :grin:

Now, you can take the input and minimise it with `cargo fuzz tmin -s none /path/to/testcase`. This will give you a more representative testcase that you can then report via the issues.

---

_Comment by @addisoncrump on 2024-01-09 14:48_

As an example, I was able to (very quickly) reproduce your discovery, and minimised it to the following:
```py
a=b=c,d,=e
```

A more realistic case:
```py
a=b=(c,d,)=e
```

Ultimately, it seems that specifically when chaining assignments, tuples that cause multiline expansion (due to trailing comma) cause the intermediary chained assignment to be wrapped with parentheses.

---

_Comment by @addisoncrump on 2024-01-09 15:45_

:sweat_smile: Revenge of the comment:
```py
(~~#
R)
```

```diff
--- Formatted Once
+++ Formatted Twice
@@ -1,5 +1,4 @@
 (
-    ~
     #
-    ~R
+    ~~R
 )
```

Similarly:
```py
[c%s#
[c]]
```

```diff
--- Formatted Once
+++ Formatted Twice
@@ -1,4 +1,3 @@
 [
-    c
-    % s[c]  #
+    c % s[c]  #
 ]
```

At a guess, empty end line comments are not idempotent when placed between pre- and post-fix operators inside brackets.

---

_Comment by @addisoncrump on 2024-01-09 15:50_

I'll stop adding issues here. To give you a sense of how I tend to go about this to help you optimise your process, I launch the fuzzers like so:

```
cargo fuzz run --sanitizer=none ruff_formatter_idempotency -- -timeout=5 -fork=1 -ignore_crashes=1 -ignore_timeouts=1
```

Then, in a second window, I launch an inotify task (inside `fuzz/artifacts`):
```sh
inotifywait -m * -e create --format "%w%f" 2>/dev/null | while read crash; do
  ls -l "$crash"
  xxd "$crash"
done
```

You can then visually inspect each crash and determine which are appropriate to minimise: 
```
./fuzz/target/x86_64-unknown-linux-gnu/release/ruff_formatter_idempotency path/to/testcase -artifact_prefix=/tmp/ -minimize_crash=1 -runs=$((1 << 16)) -mutate_depth=1
```

Then report!

---

_Marked ready for review by @manunio on 2024-01-10 11:48_

---

_Closed by @manunio on 2024-01-10 12:20_

---

_Reopened by @manunio on 2024-01-10 12:38_

---

_Comment by @MichaReiser on 2024-01-10 12:45_

@addisoncrump would you mind taking a look. I don't really know how our fuzzers work :sweat_smile: 

---

_Comment by @addisoncrump on 2024-01-10 12:47_

Sure thing, though I warn you that I wrote one of them :joy: My review might be a bit biased.

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:76 on 2024-01-10 12:48_

Could we show the lint rule in the diagnostic message? Same for the message below.

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:29 on 2024-01-10 12:48_

I don't think we need this because we aren't applying linter fixes.

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:98 on 2024-01-10 12:49_

Can you explain the motivation for calling `test_snippet` here? 

---

_@MichaReiser reviewed on 2024-01-10 12:49_

Nice

---

_Review comment by @addisoncrump on `fuzz/README.md`:108 on 2024-01-10 12:50_

```suggestion
This fuzz harness ensures that the formatter is [idempotent](https://en.wikipedia.org/wiki/Idempotence)
which detects possible unsteady states of Ruff's formatter.
```

---

_@MichaReiser reviewed on 2024-01-10 12:51_

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:27 on 2024-01-10 12:51_

Should we test with all rules (rather than just the default rule set)? 

If so, we then need to exclude the rules that we know are conflicting with the formatter

https://github.com/astral-sh/ruff/blob/da8a3af52409a5cd5034060a482ae93ac6abc27f/crates/ruff_cli/src/commands/format.rs#L735-L746

---

_Review comment by @addisoncrump on `fuzz/README.md`:113 on 2024-01-10 12:53_

```suggestion
This fuzz harness checks that Ruff's formatter does not introduce new linter errors/warnings by
linting once, counting the number of each error type, then formatting, then linting again and
ensuring that the number of each error type does not increase across formats. This has the
beneficial side effect of discovering cases where the linter does not discover a lint error when
it should have due to a formatting inconsistency.
```

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:99 on 2024-01-10 12:54_

```suggestion
```

Whoops, looks like I forgot to remove this the first time.

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:29 on 2024-01-10 12:54_

```suggestion
```

This can be removed as the later test call is not necessary.

---

_@addisoncrump reviewed on 2024-01-10 12:54_

---

_@addisoncrump reviewed on 2024-01-10 12:58_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:27 on 2024-01-10 12:58_

Can we make this a public array so that we can remove them in the fuzzer? Then, they can be updated in the crate according to when the rules are eventually made compatible, if ever.

---

_@addisoncrump reviewed on 2024-01-10 12:59_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:29 on 2024-01-10 12:59_

Nope, artifact of me misreading my own code. This can be removed, see my review.

---

_@addisoncrump reviewed on 2024-01-10 12:59_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:98 on 2024-01-10 12:59_

See above.

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:76 on 2024-01-10 12:59_

Is that not shown by `{msg:?}`?

---

_@addisoncrump reviewed on 2024-01-10 12:59_

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:76 on 2024-01-11 07:55_

Ah right, I missed this. Thanks

---

_Review comment by @MichaReiser on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:27 on 2024-01-11 07:55_

We probably could. Let's go with the standard rules for now. Is it possible to parametrize fuzzers? 

---

_@MichaReiser reviewed on 2024-01-11 07:55_

---

_Merged by @MichaReiser on 2024-01-11 07:56_

---

_Closed by @MichaReiser on 2024-01-11 07:56_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_formatter_validity.rs`:27 on 2024-01-11 11:09_

@MichaReiser Kind of, but fuzzers should generally avoid it. Since we are already parameterised over input, parameterising over something else means we basically square our input space (which is going to make the fuzzer ineffective).

---

_@addisoncrump reviewed on 2024-01-11 11:09_

---

_Branch deleted on 2024-01-11 17:53_

---
