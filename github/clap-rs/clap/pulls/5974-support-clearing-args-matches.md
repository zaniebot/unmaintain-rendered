---
number: 5974
title: Support clearing args matches
type: pull_request
state: merged
author: kryvashek
labels: []
assignees: []
merged: true
base: master
head: support-clearing-args-matches
created_at: 2025-04-17T15:43:33Z
updated_at: 2025-04-18T19:08:10Z
url: https://github.com/clap-rs/clap/pull/5974
synced_at: 2026-01-07T13:12:20-06:00
---

# Support clearing args matches

---

_Pull request opened by @kryvashek on 2025-04-17 15:43_

Resolves https://github.com/clap-rs/clap/issues/5973.

This PR just provides a method suggested in the issue linked above. Use case is described there.

---

_@epage reviewed on 2025-04-18 16:39_

---

_Review comment by @epage on `clap_builder/src/parser/matches/arg_matches.rs`:1233 on 2025-04-18 16:39_

doc comments should have a one-line summary to start with

How about something like:
```suggestion
    /// Clears the values for the given `id`
    ///
    /// Alternative to [`try_remove_*`][ArgMatches::try_remove_one] when the type is not known.
    ///
    /// Returns `Err([`MatchesError`])` if the given `id` isn't valid for current `ArgMatches` instance.
    ///
    /// Returns `Ok(true)` if there were any matches with the given `id`, `Ok(false)` otherwise.
```

---

_@kryvashek reviewed on 2025-04-18 18:21_

---

_Review comment by @kryvashek on `clap_builder/src/parser/matches/arg_matches.rs`:1233 on 2025-04-18 18:21_

Agreed, made it the way you suggested in 5d357ada532d430290c2de14c918833564f12795.

---
