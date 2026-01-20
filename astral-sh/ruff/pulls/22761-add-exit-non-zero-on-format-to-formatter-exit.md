```yaml
number: 22761
title: 📝 Add --exit-non-zero-on-format to formatter exit codes section
type: pull_request
state: open
author: alejsdev
labels:
  - documentation
assignees: []
base: main
head: docs/ruff-format-flag
created_at: 2026-01-20T12:37:36Z
updated_at: 2026-01-20T15:53:23Z
url: https://github.com/astral-sh/ruff/pull/22761
synced_at: 2026-01-20T16:46:59Z
```

# 📝 Add --exit-non-zero-on-format to formatter exit codes section

---

_@alejsdev_

## Summary

The formatter's exit codes documentation at https://docs.astral.sh/ruff/formatter/#exit-codes doesn't mention the `--exit-non-zero-on-format` flag, even though it exists (it was added in https://github.com/astral-sh/ruff/pull/16009) and appears in ruff format --help, and the linter flag --exit-non-zero-on-fix is properly documented in the https://docs.astral.sh/ruff/linter/#exit-codes (which was used as a reference to add the proper section in the formatter, trying to keep the consistency)

---

_Label `documentation` added by @AlexWaygood on 2026-01-20 12:46_

---

_@MichaReiser reviewed on 2026-01-20 15:37_

---

_Review comment by @MichaReiser on `docs/formatter.md`:379 on 2026-01-20 15:37_

I have a slight preference to extend the list above over adding this section. It does make it clearer which exit code it changes. 

```
`0` if Ruff terminates successfully, and no files would be formatted if `--check` and `--exit-non-zero-on-format` were not specified.
- `1` if Ruff terminates successfully, and one or more files would be formatted if `--check` or `--exit-non-zero-on-format` were specified.
```


It also seems that the if `--check` was not specified for 1, might be incorrect, or do I misunderstand the flags @ntBre ?

---

_@ntBre reviewed on 2026-01-20 15:53_

---

_Review comment by @ntBre on `docs/formatter.md`:379 on 2026-01-20 15:53_

I'm not sure I understand the question entirely, but I did some manual testing, and it seems like the flags work together as I would expect:

```shell
# no flags, reformatted, exits 0
❯ echo 'def foo(x,y,z): return x+y+z' > try.py && ruff format; echo $?
1 file reformatted
0
# --check, would reformat, exits 1
❯ echo 'def foo(x,y,z): return x+y+z' > try.py && ruff format --check; echo $?
Would reformat: try.py
1 file would be reformatted
1
# --exit-non-zero-on-format, reformatted, exits 1
❯ echo 'def foo(x,y,z): return x+y+z' > try.py && ruff format --exit-non-zero-on-format; echo $?
1 file reformatted
1
# --check AND --exit-non-zero-on-format, would reformat, still exits 1
❯ echo 'def foo(x,y,z): return x+y+z' > try.py && ruff format --check --exit-non-zero-on-format; echo $?
Would reformat: try.py
1 file would be reformatted
1
```

I think `--exit-non-zero-on-format` might fit nicely in the first list:

> `ruff format` exits with the following status codes:
> 
> - `0` if Ruff terminates successfully, regardless of whether any files were formatted.
> - `2` if Ruff terminates abnormally due to invalid configuration, invalid CLI options, or an
>     internal error.

as:

- `1` if Ruff terminates successfully, one or more files were formatted, and `--exit-non-zero-on-format` was specified

---
