```yaml
number: 18345
title: "Let `--output-format` update previous values"
type: issue
state: closed
author: dmyersturnbull
labels:
  - question
  - cli
assignees: []
created_at: 2025-05-27T19:55:27Z
updated_at: 2025-05-28T21:43:13Z
url: https://github.com/astral-sh/ruff/issues/18345
synced_at: 2026-01-12T15:54:56Z
```

# Let `--output-format` update previous values

---

_@dmyersturnbull_

### Summary

Ruff complains if `--output-format` is passed multiple times, including if given the same value:

```prompt
$ ruff check --fix --no-fix --fix --no-fix
# (Works as expected.)

$ ruff check --output-format concise --output-format concise
error: the argument '--output-format <OUTPUT_FORMAT>' cannot be used multiple times
```

This can be a limitation in contexts that provide the subcommand and initial options.
This showed up in a [just](https://just.systems) recipe, which was approximately:

```justfile
fix-ruff-issues *args: (ruff-check "--fix-only" args)
show-ruff-issues *args: (ruff-check "--no-fix" args)

ruff-check *args:
  ruff check --output-format grouped {{args}}
```

A user noticed a bug with my recipe:

```bash
$ just show-ruff-issues --statistics
ruff failed
  Cause: Unsupported serialization format for statistics: Grouped
# (Ok, fair enough.)

$ ruff check --statistics --output-format grouped
error: the argument '--output-format <OUTPUT_FORMAT>' cannot be used multiple times
```

Unfortunately, there's no way to remove/replace the initial value. While I can get around this in a Just recipe, it might be impossible in a Makefile or a [pre-commit](https://pre-commit.com) hook, etc.

### Version

0.11.11

---

_Label `question` added by @MichaReiser on 2025-05-28 06:13_

---

_Label `cli` added by @MichaReiser on 2025-05-28 06:14_

---

_Comment by @MichaReiser on 2025-05-28 06:17_

Hi @dmyersturnbull 

I think we should look at this in general. It isn't specific about `--output-format`. The question here is if the CLI should allow repeating arbitrary options.

I'd say no. I would expect that `--output-format concise --output-format full` would write multiple output formats (e.g. one to stderr and the other to stdout?). Since this isn't the case, I want Ruff to warn if I accidentially repeated the option (which is probably unintentional).

Fortunately, there is a workaround for your use case. You can use the `--config` option. It has lower precedence than any explicitly named argument, and it can be arbitrarily repeated. 

```shell
ruff check --config "output-format=concise" --output-format full
```

Here, `--output-format full` wins over `--config "output-format=concise"` but Ruff will use `concise` if `--output-format` isn't specified. 

An other alternative is to configure the `output-format` in your `pyproject.toml` or `ruff.toml`.

---

_Comment by @dmyersturnbull on 2025-05-28 21:13_

@MichaReiser Thanks for your reply! I didn't know about `--config`, which does help.

As for the general question of allowing repeated options, `--config` presumably obviates the need. I can understand choosing to disallow repeated options (though, admittedly, allowing `--fix --no-fix` feels inconsistent).

Your work and Astral's is very appreciated, by the way. uv in particular is a godsend.

---

_Closed by @dmyersturnbull on 2025-05-28 21:14_

---
