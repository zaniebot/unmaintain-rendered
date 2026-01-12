```yaml
number: 11063
title: "ruff format with quote-style = \"preserve\" should not show ICS001 and flake8-quotes.multiline-quotes warnings"
type: issue
state: closed
author: 57an
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-04-21T04:02:10Z
updated_at: 2024-05-22T04:31:04Z
url: https://github.com/astral-sh/ruff/issues/11063
synced_at: 2026-01-12T15:54:50Z
```

# ruff format with quote-style = "preserve" should not show ICS001 and flake8-quotes.multiline-quotes warnings

---

_@57an_

While running ruff format with quote-style = "preserve" option I get such warnings

```
warning: The following rules may cause conflicts when used with the formatter: `ISC001`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: The `flake8-quotes.multiline-quotes="single"` option is incompatible with the formatter. We recommend disabling `Q001` when using the formatter, which enforces double quotes for multiline strings. Alternatively, set the `flake8-quotes.multiline-quotes` option to `"double"`.`
```

I think with quote-style = "preserve" options ruff should not show this warnings, because this option forces string quotes format to be unchanged.

---

_Label `bug` added by @MichaReiser on 2024-04-21 11:16_

---

_Label `help wanted` added by @MichaReiser on 2024-04-21 11:16_

---

_Comment by @MichaReiser on 2024-04-21 11:18_

I agree that the `flake8-quotes.multiline-quotes` should not be shown. However, the option `quote-style: "preserve"` does not resolve the incompatibility to `ISC001` as you can see here https://play.ruff.rs/569c1110-6903-4622-8694-157b243ea73d where the second string is marked as violating ISC001 (you may need to make a small whitespace change for the warning to show up)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-22 04:26_

---

_Closed by @charliermarsh on 2024-05-22 04:31_

---
