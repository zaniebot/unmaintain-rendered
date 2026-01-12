```yaml
number: 1872
title: Implement flake8-commas
type: pull_request
state: merged
author: bluetech
labels: []
assignees: []
merged: true
base: main
head: flake8-commas
created_at: 2023-01-14T17:50:51Z
updated_at: 2023-01-29T01:19:01Z
url: https://github.com/astral-sh/ruff/pull/1872
synced_at: 2026-01-12T15:55:07Z
```

# Implement flake8-commas

---

_@bluetech_

Implements [flake8-commas](https://github.com/PyCQA/flake8-commas). Fixes #1058.

The plugin is mostly redundant with Black (and also deprecated upstream), but very useful for projects which can't/won't use an auto-formatter. 

This linter works on tokens. Before porting to Rust, I cleaned up the Python code ([link](https://gist.github.com/bluetech/7c5dcbdec4a73dd5a74d4bc09c72b8b9)) and made sure the tests pass. In the Rust version I tried to add explanatory comments, to the best of my understanding of the original logic.

Some changes I did make:

- Got rid of rule C814 - "missing trailing comma in Python 2". Ruff doesn't support Python 2.
- Merged rules C815 - "missing trailing comma in Python 3.5+" and C816 - "missing trailing comma in Python 3.6+" into C812 - "missing trailing comma". These Python versions are outdated, didn't think it was worth the complication.
- Added autofixes for C812 and C819.

Autofix is missing for C818 - "trailing comma on bare tuple prohibited". It needs to turn e.g. `x = 1,` into `x = (1, )`, it's a bit difficult to do with tokens only, so I skipped it for now.

I ran the rules on cpython/Lib and on a big internal code base and it works as intended (though I only sampled the diffs).

---

_Review comment by @charliermarsh on `src/rules/mod.rs`:9 on 2023-01-14 22:35_

Can we make this `pub(crate)` for now? (The `pub mod` pieces here have to expose `settings` as-is.)

---

_Review comment by @charliermarsh on `src/rules/flake8_commas/rules.rs`:93 on 2023-01-14 22:36_

Nit: can we call this `type_`? It's admittedly subjective, but that's the property used in several of the AST structs, so I prefer consistency.

---

_@charliermarsh reviewed on 2023-01-14 22:38_

This looks great! Really appreciate the thoroughness here.

They only major point of discussion for me is whether we should use a different error code prefix -- e.g., `COM`, like `COM812` instead of `C812`. It's admittedly inconvenient to deviate from the existing plugin, but as-is, users that have `"C"` enabled will turn this on by default unintentionally. I've been trying to avoid collision prefixes for that reason (e.g., `flake8-tidy-imports` became `TID` rather than `I`, which collided with `isort`). I would prefer to use something other than `C`, but curious to get your feedback.

Pending that change, we should be able to merge this as soon as today.

---

_@not-my-profile reviewed on 2023-01-15 05:34_

---

_Review comment by @not-my-profile on `src/rules/mod.rs`:9 on 2023-01-15 05:34_

Since #1874 which made `ruff::rules` private all of the modules in `src/rules/mod.rs` should be defined with `pub mod` since `pub(crate) mod` would be redundant.

---

_Comment by @bluetech on 2023-01-15 09:09_

Thanks for the review, I made the requested changes.

---

_Merged by @charliermarsh on 2023-01-15 19:03_

---

_Closed by @charliermarsh on 2023-01-15 19:03_

---

_Comment by @charliermarsh on 2023-01-15 19:03_

Thank you!

---

_Comment by @Davrosh on 2023-01-29 00:14_

> This looks great! Really appreciate the thoroughness here.
> 
> They only major point of discussion for me is whether we should use a different error code prefix -- e.g., `COM`, like `COM812` instead of `C812`. It's admittedly inconvenient to deviate from the existing plugin, but as-is, users that have `"C"` enabled will turn this on by default unintentionally. I've been trying to avoid collision prefixes for that reason (e.g., `flake8-tidy-imports` became `TID` rather than `I`, which collided with `isort`). I would prefer to use something other than `C`, but curious to get your feedback.
> 
> Pending that change, we should be able to merge this as soon as today.

@charliermarsh I've been searching this repo and have come across this comment in response to a failure by both ruff@0.0.236 (with --fix) and the corresponding VSCode extension to provide autofix for COM errors.
The repo states that the `fixable` option configurable via `pyproject.toml` has a default value which contains "C".
https://github.com/charliermarsh/ruff#fixable

I may be misreading this, and if so please say so, but it looks like the above comment is hinting that users that have "C" enabled will have flake8-comments enabled.

Now, I have "COM", and not "C", listed under `select` (simply because it didn't seem like it would be one of the specified options) and when I have "C" listed in my `fixable` options (either through the default value or set explicitly), the autofix option is not working for flake8-comments ("COM") on either the CLI Tool or the extension.
<img width="110" alt="image" src="https://user-images.githubusercontent.com/31009042/215296504-acdfa88a-4113-4dec-a696-6490aa47f035.png">
<img width="346" alt="image" src="https://user-images.githubusercontent.com/31009042/215296515-1a7c42ed-85a7-4ae8-b247-cd0dec57ba5f.png">
```
> poetry run ruff --fix .
file.py:20:43: COM812 Trailing comma missing
Found 1 error.
```
If I, however, manually add "COM" to the list, the autofix starts working.
<img width="95" alt="image" src="https://user-images.githubusercontent.com/31009042/215296736-142c6d69-5702-41cd-a694-5b8b379dc9ac.png">
<img width="264" alt="image" src="https://user-images.githubusercontent.com/31009042/215296886-af8c42e2-2e72-4615-af5b-83b1366ea439.png">
```
> poetry run ruff --fix .
Found 1 error (1 fixed, 0 remaining).
```

Is this an OK scenario and I'm misunderstanding this or is this a bug? Should I open an issue?
Thanks.

---

_Comment by @charliermarsh on 2023-01-29 00:17_

Thanks for the thorough note! I’ll test this out later tonight and get back to you.

---

_Comment by @charliermarsh on 2023-01-29 00:18_

(In general, though, using C is not sufficient to enable the COM rules — you have to explicitly use COM. Using C will enable the flake8-comprehensions rules, but C isn’t considered a valid “prefix” for COM.)

---

_Comment by @Davrosh on 2023-01-29 00:42_

> (In general, though, using C is not sufficient to enable the COM rules — you have to explicitly use COM. Using C will enable the flake8-comprehensions rules, but C isn’t considered a valid “prefix” for COM.)

@charliermarsh Thanks, I had a feeling that it wasn't sufficient for `select`. This is why I found it odd that "C" would be listed as part of the default value for `fixable` to begin with, while flake8-comprehensions is listed as "C4" and flake9-comments as "COM".
If "C" can, for instance, be used as a substitute **only** for the flake8-comprehensions rule ("C4"), it should be more explicitly documented (maybe I've missed that).

---

_Comment by @charliermarsh on 2023-01-29 01:04_

@Davrosh - Yeah, what's wrong here is the documentation. That `fixable` default list _should_ include `COM`. I'll fix it now.

---

_Comment by @charliermarsh on 2023-01-29 01:19_

Updated in #2316.

---
