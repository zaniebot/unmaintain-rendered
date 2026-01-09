---
number: 10430
title: "[Feature request] Support Github output for `ruff format`"
type: issue
state: closed
author: henribru
labels:
  - formatter
  - wish
assignees: []
created_at: 2024-03-16T10:09:30Z
updated_at: 2025-09-30T16:04:17Z
url: https://github.com/astral-sh/ruff/issues/10430
synced_at: 2026-01-07T13:12:15-06:00
---

# [Feature request] Support Github output for `ruff format`

---

_Issue opened by @henribru on 2024-03-16 10:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
It would be neat if `ruff format` supported `--output-format github` like `ruff check` does so that you can get annotations if you're running `ruff format --check` in Github Actions. It's not that hard to work around the lack of this with a problem matcher, but it's pretty clunky compared to the built-in support in `ruff check`:
```
      - name: Register problem matcher
        run: echo "::add-matcher::.github/workflows/matchers/ruff.json"

      - name: Run ruff format
        run: poetry run ruff format --check .
```

```
# ruff.json
{
  "problemMatcher": [
    {
      "owner": "ruff",
      "pattern": [
        {
          "regexp": "^(Would reformat): (.+)$",
          "message": 1,
          "file": 2
        }
      ]
    }
  ]
}
```

---

_Label `formatter` added by @AlexWaygood on 2024-03-16 10:10_

---

_Label `wish` added by @MichaReiser on 2024-03-18 07:57_

---

_Referenced in [astral-sh/ruff-action#12](../../astral-sh/ruff-action/issues/12.md) on 2024-10-16 19:38_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-11 16:25_

---

_Comment by @mschoettle on 2025-03-11 19:37_

I wanted to create the same issue. Ran into this on GitHub and found it via the `ruff-action` issue tracker.

@AlexWaygood I might be able to take a stab at this. What would be the correct design for this fix? Taking a quick look at the code (I've never used Rust before) I wonder if it would be sufficient to add `--output-format` to `format`. It seems that the output format influences the behaviour of `writeln!`.

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-03-11 19:56_

---

_Comment by @MichaReiser on 2025-03-12 08:06_

 Hi @mschoettle 

Yes, we'd have to add an `--output-format` option. Our UX concern here is that the formatter and linter won't support the output formats which might be sort of confusing. 

@BurntSushi is currently working on a new diagnostic infrastructure, and we're getting close to using it in Ruff, too. It should allow us to use the same diagnostics for the formatter and linter, and we should then get `--output-format` for free in the formatter. That's why I'm inclined to wait for @BurntSushi's refactor before adding this option.

---

_Comment by @alicederyn on 2025-04-30 08:59_

JSON output would also be very helpful for other integrations!

---

_Referenced in [astral-sh/ruff#19972](../../astral-sh/ruff/issues/19972.md) on 2025-08-18 16:00_

---

_Comment by @ntBre on 2025-09-30 16:04_

I just merged https://github.com/astral-sh/ruff/pull/20443, which adds support for all of the `--output-format` options in the linter to the formatter, in preview for now. It should be out in the next release on Thursday! Please give it a try and let us know if you run into any issues :)

---

_Closed by @ntBre on 2025-09-30 16:04_

---
