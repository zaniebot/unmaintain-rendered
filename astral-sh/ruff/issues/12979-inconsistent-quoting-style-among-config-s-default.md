```yaml
number: 12979
title: "Inconsistent quoting style among `config`'s default values"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - cli
assignees: []
created_at: 2024-08-19T07:34:34Z
updated_at: 2024-08-19T08:02:57Z
url: https://github.com/astral-sh/ruff/issues/12979
synced_at: 2026-01-12T15:54:52Z
```

# Inconsistent quoting style among `config`'s default values

---

_@InSyncWithFoo_

Currently some default string(-like) values are quoted, some aren't:

```shell
$ ruff config format.docstring-code-line-length
...
Default value: "dynamic"
Type: int | "dynamic"
...
```

```shell
ruff config format.indent-style
...
Default value: space
Type: "space" | "tab"
...
```

Quoted:

* [`output-format`](https://docs.astral.sh/ruff/settings/#output-format)
* [`target-version`](https://docs.astral.sh/ruff/settings/#target-version)
* [`format.docstring-code-line-length`](https://docs.astral.sh/ruff/settings/#format_docstring-code-line-length)
* [`lint.dummy-variable-rgx`](https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx)
* [`lint.flake8-quotes.docstring-quotes`](https://docs.astral.sh/ruff/settings/#lint_flake8-quotes_docstring-quotes)
* [`lint.flake8-quotes.inline-quotes`](https://docs.astral.sh/ruff/settings/#lint_flake8-quotes_inline-quotes)
* [`lint.flake8-quotes.multiline-quotes`](https://docs.astral.sh/ruff/settings/#lint_flake8-quotes_multiline-quotes)
* [`lint.flake8-tidy-imports.ban-relative-imports`](https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_ban-relative-imports)

Unquoted:

* [`cache-dir`](https://docs.astral.sh/ruff/settings/#cache-dir)
* [`format.indent-style`](https://docs.astral.sh/ruff/settings/#format_indent-style)
* [`format.line-ending`](https://docs.astral.sh/ruff/settings/#format_line-ending)
* [`format.quote-style`](https://docs.astral.sh/ruff/settings/#format_quote-style)
* [`lint.flake8-copyright.notice-rgx`](https://docs.astral.sh/ruff/settings/#lint_flake8-copyright_notice-rgx)
* [`lint.isort.default-section`](https://docs.astral.sh/ruff/settings/#lint_isort_default-section)
* [`lint.isort.relative-imports-order`](https://docs.astral.sh/ruff/settings/#lint_isort_relative-imports-order)

---

_Label `documentation` added by @MichaReiser on 2024-08-19 07:48_

---

_Label `documentation` removed by @MichaReiser on 2024-08-19 07:48_

---

_Label `cli` added by @MichaReiser on 2024-08-19 07:48_

---

_Closed by @MichaReiser on 2024-08-19 08:02_

---
