```yaml
number: 899
title: Implement F522-F525
type: pull_request
state: merged
author: olliemath
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2022-11-25T00:40:04Z
updated_at: 2022-11-25T19:07:24Z
url: https://github.com/astral-sh/ruff/pull/899
synced_at: 2026-01-12T15:55:05Z
```

# Implement F522-F525

---

_@olliemath_

Adds checks for F522 through F525. Closes https://github.com/charliermarsh/ruff/issues/891

All of the new checks return `None` if the format string is invalid (i.e. if F521 fails) - essentially I'm treating it like a syntax error and not processing further. I don't know if there's a neater way to do this, which works even if F521 is disabled.

Most of the logic here is in the helper function `FormatSummary::try_from`, which extracts all the keywords and indexes from the format string. Taking into account nested format strings like "{spam:{eggs}}" is what makes it a bit involved, so I've added some tests.

The use of `HashSet` in the summary keeps everything O(n) - for most typical python code, I'd speculate that a `Vec` would actually be quicker, however I can imagine somebody somewhere has a format function with 1000s of arguments for their own personal reasons and I wouldn't want that to come back to bite us. Use of `HashSet` means that for F524 the missing placeholders may not be returned in the order they appear in the string literal - I don't know if that's enough of a big deal to warrant implementing something more complicated.

---

_Review comment by @charliermarsh on `src/pyflakes/checks.rs`:106 on 2022-11-25 01:28_

I'd generally prefer to make `CheckKind::StringDotFormatExtraPositionalArguments` take a `Vec<String>`, and do the joining in `body` (rather than joining here and passing the formatted strings to `CheckKind::StringDotFormatExtraPositionalArguments`.

---

_Review comment by @charliermarsh on `src/pyflakes/checks.rs`:123 on 2022-11-25 01:28_

Generally prefer to use `FxHashSet`, which is faster for this kind of data.

---

_Review comment by @charliermarsh on `src/pyflakes/checks.rs`:125 on 2022-11-25 01:29_

Are there any clones that can be avoided (here or elsewhere)? E.g., can this be `FxHashMap<&str>`, rather than `FxHashMap<String>`?

---

_Review comment by @charliermarsh on `src/pyflakes/checks.rs`:63 on 2022-11-25 01:35_

Maybe instead we have these all take `FormatSummary`, and in the checker, we have something like:

```rust
if let Ok(summary) = FormatSummary::try_from(literal) {
    if self.settings.enabled.contains(&CheckCode::F524) {
        if let Some(check) =
            pyflakes::checks::string_dot_format_missing_argument(
                value, args, keywords, location,
            )
        {
            self.add_check(check);
        }
    }
    ...
}
```

We could even gate that behind `self.settings.enabled.contains(&CheckCode::F524) | ...` to only compute it if at least one of those checks is enabled.


---

_Review comment by @charliermarsh on `src/checks.rs`:59 on 2022-11-25 01:36_

Can you run `cargo dev generate-rules-table`? It'll add these to the README automatically.

---

_@charliermarsh reviewed on 2022-11-25 01:36_

---

_@olliemath reviewed on 2022-11-25 09:48_

---

_Review comment by @olliemath on `src/pyflakes/checks.rs`:106 on 2022-11-25 09:48_

Makes sense - I'll change that

---

_Review comment by @olliemath on `src/pyflakes/checks.rs`:125 on 2022-11-25 09:50_

I'll spend some time over the weekend dialling in the perf aspect

---

_@olliemath reviewed on 2022-11-25 09:50_

---

_@olliemath reviewed on 2022-11-25 10:10_

---

_Review comment by @olliemath on `src/pyflakes/checks.rs`:63 on 2022-11-25 10:10_

Yeah, that sounds good. Gate the parsing/building and do it once.

The happy path is that `F521` and some of `F522-F525` are enabled together and it will be very rare that **only** F521 is enabled - so a little more performant in the default case to go with something like:

```rust
match FormatSummary::try_from(literal) {
    Err(e) => ..., // Add an F521 check if it's enabled
    Ok(summary) => {
        // Call the other enabled checks using the summary
    }
}
```

---

_Renamed from "Implement F522-F525" to "Draft: Implement F522-F525" by @olliemath on 2022-11-25 11:46_

---

_Comment by @olliemath on 2022-11-25 12:56_

I've pulled out the construction of `FormatSummary`, so we build it at most once, and minimised the number of clones.

I can't see a way to avoid cloning when building the failure messages, but that should hopefully be rare.

---

_Comment by @charliermarsh on 2022-11-25 14:51_

This looks great! Thank you. (And yeah, we always clone when building checks and failure messages.)

---

_@charliermarsh reviewed on 2022-11-25 14:52_

---

_Review comment by @charliermarsh on `src/pyflakes/format.rs`:32 on 2022-11-25 14:52_

I'll likely change these to `FxHashSet` too, unless they're intentionally not using that!

---

_Comment by @charliermarsh on 2022-11-25 14:52_

I assume this is good to go? Anything else from your perspective @olliemath?

---

_@olliemath reviewed on 2022-11-25 15:26_

---

_Review comment by @olliemath on `src/pyflakes/format.rs`:32 on 2022-11-25 15:26_

Not intentional, just that I was unsure about the benchmarks for various data types, and the initial review pointed to the stringy sets. Fixed now.

---

_Renamed from "Draft: Implement F522-F525" to "Implement F522-F525" by @olliemath on 2022-11-25 15:26_

---

_Comment by @olliemath on 2022-11-25 15:27_

> I assume this is good to go? Anything else from your perspective @olliemath?

Yup should be good to merge assuming the pipeline passes.

---

_Comment by @olliemath on 2022-11-25 16:02_

By the way, I've been using interactive rebase to keep my commit history tidy - it looks like your general strategy is to squash+merge? In which case in future contributions I might abandon that approach and be a bit more liberal with my commits

---

_Comment by @charliermarsh on 2022-11-25 18:14_

@olliemath - Yeah, happy to elaborate but I tend towards squashing such that the Git history corresponds to "one commit per PR".


---

_Merged by @charliermarsh on 2022-11-25 18:14_

---

_Closed by @charliermarsh on 2022-11-25 18:14_

---

_Comment by @charliermarsh on 2022-11-25 18:14_

Awesome, thanks tons for this!

---

_Comment by @olliemath on 2022-11-25 19:07_

Happy to help - esp as I'm super excited about using ruff myself

---
