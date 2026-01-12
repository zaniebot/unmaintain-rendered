```yaml
number: 1885
title: "Turn define_rule_mapping! into a procedural macro"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: define_rule_mapping-proc-macro
created_at: 2023-01-15T05:11:43Z
updated_at: 2023-01-15T06:54:57Z
url: https://github.com/astral-sh/ruff/pull/1885
synced_at: 2026-01-12T05:36:32Z
```

# Turn define_rule_mapping! into a procedural macro

---

_Pull request opened by @not-my-profile on 2023-01-15 05:11_

define_rule_mapping! was previously implemented as a declarative macro, which was however was partially relying on an origin_by_code! proc macro because declarative macros cannot match on substrings of identifiers.

Currently all define_rule_mapping! lines look like the following:

    TID251 => violations::BannedApi,
    TID252 => violations::BannedRelativeImport,

We want to break up violations.rs, moving the violation definitions to the respective rule modules. To do this we want to change the previous lines to:

    TID251 => rules::flake8_tidy_imports::banned_api::BannedApi,
    TID252 => rules::flake8_tidy_imports::relative_imports::RelativeImport,

This however doesn't work because the define_rule_mapping! macro is currently defined as:

    ($($code:ident => $mod:ident::$name:ident,)+) => { ... }

That is it only supported $module::$name but not longer paths with multiple modules. While we could define `=> $path:path`[1] then we could no longer access the last path segment, which we need because we use it for the DiagnosticKind variant names. And `$path:path::$last:ident` doesn't work either because it would be ambiguous (Rust wouldn't know where the path ends ... so path fragments have to be followed by some punctuation/keyword that may not be part of paths).

So we just convert the declarative macro into a procedural macro in order to support paths of arbitrary length.

[1]: https://doc.rust-lang.org/reference/macros-by-example.html#metavariables

---

_Comment by @not-my-profile on 2023-01-15 05:16_

I have found that GitHub apparently mangles commit messages when merging by squashing:
See 7b1ce72f862f0967df1924c583288ccc8b7cb81c which does not have the second block indented vs my original commit, which correctly indented it 5569f668fe3183f6f48c2c7f3722dee74a0c4737.

So if you don't mind, I'd prefer it if you simply merged all of my PRs via rebasing (so even PRs that only contain a single commit) ... I don't want GitHub messing up my commit messages.

(And also because I sometimes force push commit message fixes which don't make it into the PR description.)

---

_Merged by @charliermarsh on 2023-01-15 06:54_

---

_Closed by @charliermarsh on 2023-01-15 06:54_

---
