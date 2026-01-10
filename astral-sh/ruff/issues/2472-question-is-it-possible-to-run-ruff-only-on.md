```yaml
number: 2472
title: "[Question] is it possible to run ruff only on edited lines of a branch/commit?"
type: issue
state: closed
author: tharwan
labels:
  - question
assignees: []
created_at: 2023-02-02T12:33:04Z
updated_at: 2025-03-28T13:09:00Z
url: https://github.com/astral-sh/ruff/issues/2472
synced_at: 2026-01-10T11:09:45Z
```

# [Question] is it possible to run ruff only on edited lines of a branch/commit?

---

_Issue opened by @tharwan on 2023-02-02 12:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Similar to the what darker does for black, it would be very nice for introducing ruff to a code base to only apply it to changed lines.

---

_Comment by @charliermarsh on 2023-02-02 13:06_

Hey! It's not possible right now -- some things like this have come up in #1149, #1931, #2189. I'm hesitant to support it since it's a hard feature to get right. I haven't used `darker`, but the problem with applying that methodology to Ruff (IIUC) is that unlike code formatting, linter errors are not always localized: you could remove an import from one part of the file, and it could cause an error in an unchanged part of the file.

One alternative (not what you're looking for, but thought I'd mention it) is that you can run `ruff --add-noqa /path/to/file.py` and it will add `noqa` suppressions to any lines that fail. I know this won't work in all cases, but it's an option.


---

_Closed by @charliermarsh on 2023-02-02 13:06_

---

_Label `question` added by @charliermarsh on 2023-02-02 13:06_

---

_Comment by @tharwan on 2023-02-02 13:24_

Thanks for the quick reply! I guess another option would be to include all files touched by  a commit.

---

_Comment by @charliermarsh on 2023-02-02 13:40_

Yeah definitely! Although I feel like there are probably ways to do that via Git commands and piping, and if they're workable, it's a bit easier than building logic into Ruff.

In Flake8, they suggest this as an alternative to their `--diff` functionality which was deprecated:

```
git diff --name-only -z --diff-filter=d -- '*.py' | xargs -0 flake8 or utilize pre-commit run --from-ref=... --to-ref=...
```

But I haven't tried it.

---

_Comment by @itolosa on 2024-01-05 20:31_

I guess another brute force solution would be to run ruff before and after the changes and in some sort do a set difference:

introducedErrors = errorsAfter - errorsBefore

---

_Comment by @victor-varjo on 2025-03-28 13:09_

> I guess another brute force solution would be to run ruff before and after the changes and in some sort do a set difference:
> 
> introducedErrors = errorsAfter - errorsBefore

And make it aware of file renames.

---
