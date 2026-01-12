```yaml
number: 2546
title: "[readme.md] Automated link to pylint documentation using pre-commit"
type: pull_request
state: closed
author: Pierre-Sassoulas
labels: []
assignees: []
base: main
head: automated-link-to-pylint-doc
created_at: 2023-02-03T15:53:46Z
updated_at: 2023-02-08T02:40:08Z
url: https://github.com/astral-sh/ruff/pull/2546
synced_at: 2026-01-12T15:55:08Z
```

# [readme.md] Automated link to pylint documentation using pre-commit

---

_@Pierre-Sassoulas_

This permit to have a link to the pylint documentation explaining the message using a pre-commit hook. Result here: https://github.com/Pierre-Sassoulas/ruff/tree/automated-link-to-pylint-doc#pylint-pl

---

_@Pierre-Sassoulas reviewed on 2023-02-03 15:59_

---

_Review comment by @Pierre-Sassoulas on `README.md`:1354 on 2023-02-03 15:59_

The real name seems to be ``consider-using-sys-exit``, there's other broken links, I'll fix them if the PR is deemed valuable. (Otherwise I can fix in another MR)

---

_Comment by @JonathanPlasse on 2023-02-03 16:04_

Not everyone uses `pre-commit` alas.
So, you would need to integrate it inside `cargo dev generate-all` as it centralizes the code generation and the CI would check if the generated code is up to date.

---

_Review comment by @charliermarsh on `README.md`:1354 on 2023-02-03 16:28_

Okay cool -- we can safely rename these as they aren't part of the public API in any way yet.

---

_@charliermarsh reviewed on 2023-02-03 16:28_

---

_Comment by @charliermarsh on 2023-02-03 16:28_

Thanks for getting involved :) Happy to incorporate this but it does likely need to be part of `generate-all`.

---

_Comment by @Pierre-Sassoulas on 2023-02-03 23:41_

Any tip on how to make [``rule.as_ref()``](https://github.com/charliermarsh/ruff/blob/ff859ead852b24cf6b2473a798678ccf049943c4/ruff_dev/src/generate_rules_table.rs#L38) be a link only for pylint's rule ? I'm new to rust I don't see where the function needs to be modified, I think it's generic for all Rules.

---

_Comment by @not-my-profile on 2023-02-04 03:38_

Couple of notes:

1. We do not want to blindly rename our violations to match Pylint. As I said in #1773 we want instead want to follow the [Rust naming convention for lints](https://rust-lang.github.io/rfcs/0344-conventions-galore.html#lints):

   > the lint name should make sense when read as "allow lint-name" or "allow lint-name items"

   So `use-sys-exit` should actually be called `use-of-exit-or-quit`.

   The documentation of pylint messages only appears to be linkable via the pylint symbolic name ... we do not currently track these ... I think that's very much a blocker for implementing this (unless pylint introduces some redirect endpoint that redirects from the pylint code to the respective documentation).

2. We certainly want to implement this in a way that also works for other linters.

> Any tip on how to make [rule.as_ref()](https://github.com/charliermarsh/ruff/blob/ff859ead852b24cf6b2473a798678ccf049943c4/ruff_dev/src/generate_rules_table.rs#L38) be a link only for pylint's rule ? I'm new to rust I don't see where the function needs to be modified, I think it's generic for all Rules.

`as_ref` is much more generic than that ... since it's an interface from [the `AsRef` trait of the standard library](https://doc.rust-lang.org/std/convert/trait.AsRef.html). In this case it just returns a `&str`.

I am afraid that this is not really a beginner friendly issue, if you're new to Rust, you'll probably have a better experience trying to do something from [the issues labeled as `good first issue`](https://github.com/charliermarsh/ruff/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22).

---

_Comment by @charliermarsh on 2023-02-04 03:44_

For what it's worth: it's fine to rename the Pylint rules to match upstream for now. We do it for _most_ of them, and that's the current _intent_ for that rule set, so any that deviate are an error. We need to do a pass to rename all rules consistently anyway prior to making that a public API.

In other words: as long as we aren't applying `"allow lint-name"` consistently across Ruff, then having Pylint rules be consistent with upstream is preferable to having them be inconsistent with upstream. So I'm fine committing those changes. (I won't be working on them personally, but I will certainly merge them.)


---

_Comment by @not-my-profile on 2023-02-04 03:52_

I'd rather avoid needlessly naming rules back and forth. Many of the pylint rules are implemented also by flake8 plugins so naming them after pylint is rather arbitrary.

---

_Comment by @Pierre-Sassoulas on 2023-02-04 08:02_

Maybe we can:
- Add the links in post-process after the Readme is generated (?) If so I can more or less adapt the python script to rust
- Have a 1-n relationship between ruff messages and other linter's messages (in the future for pylint, as the renaming was merged in #2559). That would also be convenient outside of ruff's repository if it's a json or something that can be parsed to generate doc automatically here and elsewhere.

---

_Comment by @charliermarsh on 2023-02-08 02:40_

Let's omit this for now. We now have the ability to write documentation inline (see: https://github.com/charliermarsh/ruff/issues/1467#issuecomment-1421613021), so we'll bring these over as first-class documentation in Ruff itself (see: https://github.com/charliermarsh/ruff/issues/2646).

---

_Closed by @charliermarsh on 2023-02-08 02:40_

---
