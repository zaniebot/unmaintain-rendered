```yaml
number: 4343
title: Expose explanation for a code
type: issue
state: closed
author: jankatins
labels:
  - cli
assignees: []
created_at: 2023-05-10T10:26:59Z
updated_at: 2023-05-11T23:22:25Z
url: https://github.com/astral-sh/ruff/issues/4343
synced_at: 2026-01-10T11:09:47Z
```

# Expose explanation for a code

---

_Issue opened by @jankatins on 2023-05-10 10:26_

In https://github.com/koxudaxi/ruff-pycharm-plugin/issues/147 we were discussing the possibility to annotate the `noqa: <code>` codes with the example text as it is on the website. Is there a way to get this information from ruff? E.g. something like `ruff explain <code>` would return the explanation for the code (in machine readable format, e.g. `--json`)?

---

_Comment by @JonathanPlasse on 2023-05-10 12:14_

Currently, we have
```console
❯ ruff rule S101 --format=json
{
  "code": "S101",
  "linter": "flake8-bandit",
  "summary": "Use of `assert` detected"
}
```
It does not contain the explanation.

---

_Label `cli` added by @charliermarsh on 2023-05-10 14:53_

---

_Comment by @charliermarsh on 2023-05-10 15:40_

Hmm, yeah, the JSON output should probably include the documentation? Similarly to how `ruff rule S101` shows:

```markdown
# assert (S101)

Derived from the **flake8-bandit** linter.

## What it does
Checks for uses of the `assert` keyword.

## Why is this bad?
Assertions are removed when Python is run with optimization requested
(i.e., when the `-O` flag is present), which is a common practice in
production environments. As such, assertions should not be used for runtime
validation of user input or to enforce  interface constraints.

Consider raising a meaningful error instead of using `assert`.
```

---

_Comment by @JonathanPlasse on 2023-05-10 18:40_

I can work on it.

---

_Assigned to @JonathanPlasse by @MichaReiser on 2023-05-11 07:46_

---

_Comment by @jankatins on 2023-05-11 09:32_

BTW: is it intended that the non-json output never shows the summary? `Use of `assert` detected` is in `ruff rule S101 --format=json` but not in `ruff rule S101`. I would have expected that this is basically the headline: `# assert (S101): Use of 'assert' detected` or so.

---

_Comment by @JonathanPlasse on 2023-05-11 15:08_

I would be in favor of always showing `message_formats`.

---

_Comment by @charliermarsh on 2023-05-11 15:55_

The message formats are somewhat problematic because they're data-dependent -- that is, they need to be filled in based on the diagnostic context (with variable names, etc.).


---

_Closed by @charliermarsh on 2023-05-11 23:22_

---
