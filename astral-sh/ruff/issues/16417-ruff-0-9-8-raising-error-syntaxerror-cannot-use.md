```yaml
number: 16417
title: "`ruff` 0.9.8 raising error 'SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)' in Python 3.13"
type: issue
state: closed
author: aidangallagher4
labels:
  - preview
assignees: []
created_at: 2025-02-27T15:39:15Z
updated_at: 2025-02-28T08:58:12Z
url: https://github.com/astral-sh/ruff/issues/16417
synced_at: 2026-01-12T15:54:55Z
```

# `ruff` 0.9.8 raising error 'SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)' in Python 3.13

---

_@aidangallagher4_

### Summary

When running `ruff check` for the first time on a Python file (version 3.13.1) containing `match` statements (ie either when `ruff` has just been installed, or the file has just been created), the error

```python
SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
```

is returned. This error is hard to debug as it will not appear for subsequent runs of `ruff check` in the same repo (all checks pass) -- I suspect this is why I've been unable to replicate in the [playground](https://play.ruff.rs/c96ec5e7-925f-41f2-adde-22d64d8a3904)

Replicated in Python 3.12 and Python 3.13 on Mac running Sequoia 15.3.1, and for the same Python versions on a Windows machine running WSL. MWE here: [ruff-demo.zip](https://github.com/user-attachments/files/19012111/ruff-demo.zip)

As a short-term fix you can either revert to `ruff<=0.9.7` or include the line `target-version = "py313"` in your ruff config (I assume it would work for any version >3.9)

### Version

ruff 0.9.8 (568cf88c6 2025-02-27)

---

_Comment by @MichaReiser on 2025-02-27 15:43_

> This error is hard to debug as it 

Do you have any suggestions on how we could improve the error message to make it easier?

> is returned. This error is hard to debug as it will not appear for subsequent runs of ruff check in the same repo (all checks pass) -- I suspect this is why I've been unable to replicate in the [playground](https://play.ruff.rs/c96ec5e7-925f-41f2-adde-22d64d8a3904)

This is interesting. That would suggest that we fail to cache the diagnostics correctly and it also seems that we missed to add the diagnostics to the playground. We should look into both (@ntBre).

> As a short-term fix you can either revert to ruff<=0.9.7 or include the line target-version = "py313" in your ruff config (I assume it would work for any version >3.9)

Yes, setting the `target-version` to the python version you use (or use `requires-python`) is the right fix. It also ensures that the formatter and linter assume the right python version.

---

_Label `preview` added by @MichaReiser on 2025-02-27 15:44_

---

_Comment by @ntBre on 2025-02-27 15:46_

Thanks @MichaReiser. Just to confirm, you are using `--preview` (or an equivalent config option) right? The new errors should only be active on preview.

The caching thing is definitely surprising to me. I'll look into that and the playground stuff today.

---

_Comment by @ntBre on 2025-02-27 15:47_

Ah okay I see `preview = true` in the included config, thanks!

---

_Assigned to @ntBre by @ntBre on 2025-02-27 15:50_

---

_Comment by @aidangallagher4 on 2025-02-27 15:54_

Hey @MichaReiser, @ntBre, thanks for the swift response. To reply to specifics:

>> This error is hard to debug as it

>Do you have any suggestions on how we could improve the error message to make it easier?

the error message is perfectly clear, it's just occurring in an invalid context (since I am on Python 3.13)

Is it the case that `target-version` is required so `ruff` is aware of the error (so it is defaulting to below 3.10) or is there some other version-awareness present? If it _is_ the case that it's required then the only issue is the fact that the error _doesn't_ occur again :) If it _isn't_ the case that it's required then I guess version-awareness is broken somehow?

---

_Comment by @ntBre on 2025-02-27 16:20_

Yeah, I think `target-version` or `requires-python` is currently necessary to specify a non-default value (we currently default to 3.9), but we may need to update that behavior, especially combined with these new errors. With that said, I think you're right that the error here is that it doesn't occur again 😆 . I'll work on that caching and the playground integration for this issue and open a separate issue to discuss the version-awareness. It seems like we could do something better there too.

---

_Comment by @MichaReiser on 2025-02-27 16:57_

This is probably somewhat involved but something that could help here as well is to explain why Ruff assumes Python 3.9. 

E.g. *Python 3.9 is the default* or *3.9 is configured in `ruff.toml`* etc. Clippy does something like 

---

_Comment by @Thell on 2025-02-28 03:00_

I just wanted to add that I am also getting this `match` statement syntax error on a fresh extensions setup (vscode) with Python 3.12.9 and `requires-python = ">=3.12"` in my `pyproject.toml`. And this is __not__ using insiders, preview or anything like that.

---

_Comment by @charliermarsh on 2025-02-28 03:01_

Do you have a separate `ruff.toml`?

---

_Comment by @Thell on 2025-02-28 03:03_

> Do you have a separate `ruff.toml`?

No. Do you want me to add one with anything specific to test something specific?

---

_Comment by @Thell on 2025-02-28 03:05_

```toml
# Assume Python 3.12
target-version = "py312"
```

That removed the error. I do hope that isn't a 'fix' and is just a work-around.

---

_Comment by @ntBre on 2025-02-28 03:08_

Hmm, thanks for this report. Maybe it's not properly gated behind `--preview` in the editor. I also thought it would respect `requires-python`, but I guess it's only looking at `target-version`. Both of those are bugs.

---

_Comment by @ntBre on 2025-02-28 03:40_

Do you have a `[tool.ruff]` section in your `pyproject.toml`? I was able to reproduce this with this file:

```toml
[project]
requires-python = ">=3.12"
```

but adding an empty `tool.ruff` table properly loads the `requires-python` value for me, at least in the CLI:

```toml
[project]
requires-python = ">=3.12"

[tool.ruff]
```

---

_Comment by @Thell on 2025-02-28 04:04_

> Do you have a `[tool.ruff]` section in your `pyproject.toml`? I was able to reproduce this with this file:

No, I did not have a `[tool.ruff]` section. Adding the empty section and commenting out the `ruff.toml`'s `target-version` reproduces the syntax error on `match`, but deleting the `roff.toml` completely while leaving the empty section induces a proper usage of the project's `requires-python`.


---

_Comment by @dhruvmanila on 2025-02-28 04:14_

> but adding an empty `tool.ruff` table properly loads the `requires-python` value for me, at least in the CLI:

I think it's because unless there's a `tool.ruff` header in `pyproject.toml` we don't read the `requires-python` field in it. This means that Ruff will target `3.9` (default) even though `requires-python` says `>=3.12`. This should be resolved by https://github.com/astral-sh/ruff/pull/16319 at least.

---

_Comment by @ntBre on 2025-02-28 04:16_

Thanks @dhruvmanila, you just beat me to it 😄 This is pretty much what I was typing out:

Okay, I think that's in line with the current [config file discovery](https://docs.astral.sh/ruff/configuration/#config-file-discovery) mechanism, but we should be updating that in the next minor release to be a bit more intuitive (https://github.com/astral-sh/ruff/pull/16319). And I have a PR up to fix the preview gate, which should be out in the next patch release next week.

Thanks again for the report and for testing out all these configurations!

---

_Comment by @dhruvmanila on 2025-02-28 04:18_

> And I have a PR up to fix the preview gate, which should be out in the next patch release next week.

We can do a release after your fix is merged to get it out quickly.

---

_Closed by @MichaReiser on 2025-02-28 08:58_

---
