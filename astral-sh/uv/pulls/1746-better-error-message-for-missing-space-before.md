```yaml
number: 1746
title: Better error message for missing space before semicolon in requirements
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/missing-space-before-semicolon
created_at: 2024-02-20T11:21:48Z
updated_at: 2024-02-20T16:38:37Z
url: https://github.com/astral-sh/uv/pull/1746
synced_at: 2026-01-12T16:04:42Z
```

# Better error message for missing space before semicolon in requirements

---

_@konstin_

PEP 508 requires a space between a URL and the semicolon separating it from the markers to disambiguate it from a url ending with a semicolon. This is easy to get wrong because the space is not required after a plain name of PEP 440 specifier. The new error message explicitly points out the missing space.

Fixes #1637

---

_Label `error messages` added by @konstin on 2024-02-20 11:21_

---

_Review comment by @AlexWaygood on `crates/pep508-rs/src/lib.rs`:989 on 2024-02-20 12:20_

This tells you what's gone wrong, but not why we need to issue an error here in the first place. I think if I were a user and I got this message, my immediate question would be, "Well, you obviously understood what I _meant_ here... so why not just do that?"

Maybe something like this? Not sure if there's a more concise way of explaining it:

```suggestion
                    message: Pep508ErrorSource::String("\
Missing space before ';' creates an ambiguous requirement
(Unclear whether the ';' is part of the URL or separates the URL from an environment marker)".to_string()
                    ),
```

---

_@AlexWaygood reviewed on 2024-02-20 12:20_

Nice!

---

_@konstin reviewed on 2024-02-20 16:34_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:989 on 2024-02-20 16:34_

The reason itself is somewhat counterintuitive since we allow paths too, which unlike urls can contain spaces which we don't support except for the hack with file urls, and because PEP 508 doesn't require a semicolon after a version number for symmetry.

---

_@AlexWaygood approved on 2024-02-20 16:34_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:987 on 2024-02-20 16:36_

Can we just make this `if let` rather than `unwrap`?

---

_@charliermarsh approved on 2024-02-20 16:36_

---

_Merged by @konstin on 2024-02-20 16:38_

---

_Closed by @konstin on 2024-02-20 16:38_

---

_Branch deleted on 2024-02-20 16:38_

---
