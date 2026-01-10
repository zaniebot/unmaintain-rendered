```yaml
number: 11955
title: "ruff with `--diff` exits 0 on errors with no diffs"
type: issue
state: open
author: wyardley
labels:
  - cli
assignees: []
created_at: 2024-06-20T20:42:19Z
updated_at: 2025-12-16T16:24:47Z
url: https://github.com/astral-sh/ruff/issues/11955
synced_at: 2026-01-10T11:09:54Z
```

# ruff with `--diff` exits 0 on errors with no diffs

---

_Issue opened by @wyardley on 2024-06-20 20:42_

Using `--diff` flag with no possible changes seems to result in a 0 exit code and no message (or error), regardless of whether there are errors, formatting changes, and whether or not those are fixable. While these may not be intended to be used together (and while it is noted that `--diff` implies `--fix-only`, it wasn't immediately obvious that this behavior would happen since the docs mention reporting, but not exit status), I came across a situation where someone had used `--diff` and `--fix` together -- see https://github.com/python-semantic-release/python-semantic-release/pull/961 and https://github.com/python-semantic-release/python-semantic-release/pull/957

To me, the expected behavior would be to have a way for the tool to still exit non-0 if any findings are present. If the flags are not expected to be used together, an error to that effect (and non-0 exit) would also be fine.

Note that `--diff` on its own _does_ have the expected behavior 

I think maybe part of the issue is that `ruff --diff` is only exiting 1 if there is a diff, which is maybe sensible, just confusing in this case. Maybe we need a `--exit-non-zero-on-error` as well as the existing `--exit-non-zero-on-fix` flag and / or a clarification on this point in the docs for `--diff`?

I'm not sure what the person who set this [the pipeline above] up originally thought / wanted, but presumably they wanted to both print the changes, if any, that would be made by `ruff --fix` _and_ track whether any findings came up.

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
- "diff"
- "diff fix"

* A minimal code snippet that reproduces the bug.
```python
import os.path as OSPATH

is_path = OSPATH.isdir("/tmp")
print(f"Hello world {is_path}")
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
```shell
$ ruff check test_ruff.py --diff --fix --output-format=full --exit-non-zero-on-fix
$ echo $?
0
$ ruff check test_ruff.py --diff --fix
$ echo $?
0
```

* The current Ruff settings (any relevant sections from your `pyproject.toml`)
Not sure if it's strictly necessary, but for the above to work, a rule that will trip and won't trigger any diff (e.g., `N812`) has to be enabled.

```toml
[lint]
select = ["N812"]
```

* The current Ruff version (`ruff --version`).
`ruff 0.4.9`


---

_Renamed from "Conflict between --diff and --fix flags" to "Ruff --diff exits 0 on errors" by @wyardley on 2024-06-20 22:10_

---

_Renamed from "Ruff --diff exits 0 on errors" to "ruff with `--diff` exits 0 on errors" by @wyardley on 2024-06-20 22:10_

---

_Renamed from "ruff with `--diff` exits 0 on errors" to "ruff with `--diff` exits 0 on errors with no diffs" by @wyardley on 2024-06-20 22:17_

---

_Closed by @MichaReiser on 2024-06-21 06:13_

---

_Reopened by @MichaReiser on 2024-06-21 06:14_

---

_Label `cli` added by @MichaReiser on 2024-06-21 06:14_

---

_Comment by @MichaReiser on 2024-06-21 06:18_

Thanks for opening this issue and also creating a PR to improve the documentation. 

We share your sentiment that `--diff` is confusing and we have a few ideas on how to improve the usability but haven't had time to implement it:

* Change the default output format to show fixes. 
* Introduce a new `ruff fix` command that only applies fixes. Remove the `--diff` option from `ruff --check`. I think this should remove the confusion that you're experiencing because it's then more clear that `fix` should only fail if fixing failed, not if there are any other unfixed violations.


---

_Comment by @wyardley on 2024-06-21 06:27_

That makes sense, thanks! It took a few passes for me to untangle the layers here, hence all the edits. The behavior wasn’t quite what I originally would have expected, but it all makes sense to me now.

I agree that a separate subcommand could work, though the feature of being able to check and fix in one pass is convenient for things like pre-commit hooks, even if ruff is fast enough that running twice in and of itself shouldn’t normally matter.

Happy to either keep this open or close it as you like.

---

_Comment by @MichaReiser on 2024-06-21 08:31_

> I agree that a separate subcommand could work, though the feature of being able to check and fix in one pass is convenient for things like pre-commit hooks, even if ruff is fast enough that running twice in and of itself shouldn’t normally matter.

We consider to keep `ruff check --fix` but without the `--diff` option and the default `output-format` would show the fixes (at least for the diagnostics for which no fixes exist)

I'll keep it open because it helps us prioritize (knowing that this is something user actually ran into)

---

_Comment by @Hoolean on 2025-12-16 16:24_

Hello all! Thanks for the explanations above : )

We encountered this in the wild too: the ideal we were aiming towards was to have **all** issues be reported, but with those that were auto-fixable furnished with their diffs in the CI log too.

---
