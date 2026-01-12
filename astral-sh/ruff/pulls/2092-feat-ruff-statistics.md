```yaml
number: 2092
title: "feat: ruff --statistics"
type: pull_request
state: closed
author: seanabreau
labels: []
assignees: []
base: main
head: main
created_at: 2023-01-22T20:27:07Z
updated_at: 2023-01-29T18:45:27Z
url: https://github.com/astral-sh/ruff/pull/2092
synced_at: 2026-01-12T15:55:07Z
```

# feat: ruff --statistics

---

_@seanabreau_

adds --statistics flag from issue #1814
open to all feedback :)

---

_Review comment by @charliermarsh on `ruff_cli/src/printer.rs`:168 on 2023-01-23 04:11_

I think by default, for now, we should try to adhere to Flake8's format -- number of violations, then code, then default message, with consistent columnar width for the first column:

```
...
2     W391 blank line at end of file
8     W605 invalid escape sequence '\.'
2     W606 'async' and 'await' are reserved keywords starting with Python 3.7
2     YTT101 `sys.version[:3]` referenced (python3.10), use `sys.version_info`
2     YTT102 `sys.version[2]` referenced (python3.10), use `sys.version_info`
5     YTT103 `sys.version` compared to string (python3.10), use `sys.version_info`
4     YTT201 `sys.version_info[0] == 3` referenced (python4), use `>=`
2     YTT202 `six.PY3` referenced (python4), use `not six.PY2`
2     YTT203 `sys.version_info[1]` compared to integer (python4), compare `sys.version_info` to tuple
2     YTT204 `sys.version_info.minor` compared to integer (python4), compare `sys.version_info` to tuple
2     YTT301 `sys.version[0]` referenced (python10), use `sys.version_info`
5     YTT302 `sys.version` compared to string (python10), use `sys.version_info`
2     YTT303 `sys.version[:1]` referenced (python10), use `sys.version_info`
```

---

_Review comment by @charliermarsh on `ruff_cli/src/printer.rs`:161 on 2023-01-23 04:13_

Rather than putting this in printer, and introducing a new log level... what if, instead, we forked where we call `printer.write_once(&diagnostics)?` from `main.rs`. That is: check if we're doing statistics, then call a new `Printer::statistics` instead of `Printer::write_once`?

That way, we can also have different formats for statistics. For example, we could output JSON! And the implementation would be entirely separate from this method, which deals with a different kind of task and is already a little complex.

---

_Review comment by @charliermarsh on `ruff_cli/src/main.rs`:228 on 2023-01-23 04:14_

Nit: mind reverting this?

---

_Review comment by @charliermarsh on `ruff_cli/tests/integration_test.rs`:177 on 2023-01-23 04:14_

Thanks for this!

---

_Review comment by @charliermarsh on `src/logging.rs`:59 on 2023-01-23 04:15_

I think it'd be best not to tie this to log level. `--verbose` and `--quiet` should actually work with `--statistics`, I think, since some of that deals with debug logging that isn't related to that final output format.

---

_@charliermarsh requested changes on 2023-01-23 04:15_

Thanks for digging into this!

---

_Comment by @ngnpope on 2023-01-23 10:24_

A few thoughts that come to mind...

Would it be possible to provide the following additional flags?

- `--statistics-sort-field` accepting `count` _(default)_ and `code`
- `--statistics-sort-order` with `asc` and `desc` _(default)_

Would it make sense to split the code into a separate column to the description? In future additional columns could be possibly be added to the stats via `--statistics-fields`... For example, `filename` could be added to have a breakdown by file.

What about also accepting `--statistics-format` where there could be `text` _(default)_, `json`, `jsonl`, etc.?

I guess these could be farmed out to separate issues to break this down into smaller chunks, but the sorting should be relatively straightforward to add here?

---

_Comment by @charliermarsh on 2023-01-23 17:49_

I'd love to try and output in a format that's amenable to doing those kinds of transforms in a downstream tool, e.g. with Unix-style piping, rather than implement all the flags and options in Ruff itself. For example: `--format=json` is intended to work here, so users could always output as JSON then sort and filter themselves. But maybe there's a way to output standard tabular data that can be manipulated in that way too? I'd have to look at what some other, comparable tools for inspiration.


---

_@seanabreau reviewed on 2023-01-25 05:20_

---

_Review comment by @seanabreau on `ruff_cli/src/printer.rs`:161 on 2023-01-25 05:20_

good idea! on it!

---

_Comment by @spaceone on 2023-01-26 15:38_

I tried using it and currently get:

```
error[E0425]: cannot find value `cli` in this scope
   --> ruff_cli/src/args.rs:436:15
    |
436 |     } else if cli.statistics {
    |               ^^^ not found in this scope


```

should be `args.statistics`

---

_Comment by @spaceone on 2023-01-27 13:57_

FYI: as I am using this heavily I used your WIP version and rebased onto latest main: https://github.com/spaceone/ruff/pull/new/statistics
(in case you want to continue working on it without resolving the merge conflicts on your own).

---

_Comment by @charliermarsh on 2023-01-29 18:45_

Closing in favor of #2284 -- which just merged!

---

_Closed by @charliermarsh on 2023-01-29 18:45_

---
