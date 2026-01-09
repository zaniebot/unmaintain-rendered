---
number: 13108
title: "`ruff rule` should include a property that indicates the fix is unsafe or not"
type: issue
state: closed
author: zender-vivodyne
labels:
  - question
  - great writeup
assignees: []
created_at: 2024-08-26T12:50:16Z
updated_at: 2024-09-09T12:01:21Z
url: https://github.com/astral-sh/ruff/issues/13108
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff rule` should include a property that indicates the fix is unsafe or not

---

_Issue opened by @zender-vivodyne on 2024-08-26 12:50_

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

## Usecase

We have a legacy python project that we want to ease into using ruff and would like to start with just formatting and any linting that  can be automatically fixed. Using the `ruff rule --all --output-format json` we should be able to build a command that allows for that. 

Command we are using to build the list (tested only on MacOS)

```
ruff rule --all --output-format json | jq -r '.[] | select(.fix == "Fix is always available." and .preview == false and .code ) | .code' | sort | paste -sd, - | sed 's/[^,]*/"&"/g'
```

We can then just take this and put it into the `select` section of the config. 

Since there is no flag it requires us to copy and paste the above, then run `ruff check --fix --statistics` and copy all the values that are reported after not being fixed into the `ignore` list. 

## Current state
`ruff rule --all --output-format json`

Example output:

```
[
...
{
    "name": "verbose-raise",
    "code": "TRY201",
    "linter": "tryceratops",
    "summary": "Use `raise` without specifying exception name",
    "message_formats": [
      "Use `raise` without specifying exception name"
    ],
    "fix": "Fix is always available.",
    "explanation": "## What it does\nChecks for needless exception names in `raise` statements.\n\n## Why is this bad?\nIt's redundant to specify the exception name in a `raise` statement if the\nexception is being re-raised.\n\n## Example\n```python\ndef foo():\n    try:\n        ...\n    except ValueError as exc:\n        raise exc\n```\n\nUse instead:\n```python\ndef foo():\n    try:\n        ...\n    except ValueError:\n        raise\n```\n\n## Fix safety\nThis rule's fix is marked as unsafe, as it doesn't properly handle bound\nexceptions that are shadowed between the `except` and `raise` statements.\n",
    "preview": false
  },
...
]
```

`fix` will only be one of 3 values today. https://github.com/astral-sh/ruff/blob/f8f2e2a442aae2c1dfb4949e5ce99dc7a9d3e902/crates/ruff_diagnostics/src/violation.rs#L10-L18

## Possible options

* Update the `fix` property to add some sort of indicator on if the fix will be unsafe or not. 
* Add a new property that indicates unsafe is `true` or `false`. Similar to how `preview` is done. 
* Maybe its a new flag on the cli that `--fixable` that can be used alongside or instead of `--all`


### Keywords
unsafe fix rule property cli flag


---

_Comment by @dhruvmanila on 2024-08-26 13:17_

Thanks for providing all the details!

> We have a legacy python project that we want to ease into using ruff and would like to start with just formatting and any linting that can be automatically fixed. Using the `ruff rule --all --output-format json` we should be able to build a command that allows for that.
> 
> Command we are using to build the list (tested only on MacOS)
> 
> ```
> ruff rule --all --output-format json | jq -r '.[] | select(.fix == "Fix is always available." and .preview == false and .code ) | .code' | sort | paste -sd, - | sed 's/[^,]*/"&"/g'
> ```
> 
> We can then just take this and put it into the `select` section of the config.

I believe this can be achieved using the following command 😅:
```
ruff check --select=ALL --fix
```

Or, am I misunderstanding what you're trying to achieve?

---

_Label `question` added by @dhruvmanila on 2024-08-26 13:17_

---

_Label `great writeup` added by @dhruvmanila on 2024-08-26 13:17_

---

_Comment by @zender-vivodyne on 2024-08-26 14:58_

@dhruvmanila so running that presented me with the below output 😓 which we want to get to 0 eventually and tune it to how we setup our other projects over time. So this command you provided i think will just fix all the things but what i want is for all the fixable rules to be configured and the exit status to be 0 and not spew into the console. 

Little more details i that we plan on wiring this up via pre-commit and CI to check so that we can keep adding more rules slowly and fix things in chunks. The fixable list just gives us a workable base and some small wins in that it'll just clean them all up for us. I know `--exit-zero` also exists but that doesn't give us any confidence when we do add an unfixable rule in the future that we wont have regressions and have more code written that violate it. 

```
Found 17176 errors (226 fixed, 16950 remaining).
No fixes available (2684 hidden fixes can be enabled with the `--unsafe-fixes` option).

$ echo $?
1
```

---

_Comment by @dhruvmanila on 2024-08-30 09:10_

I see, thanks for providing the details. I think you already know this but not all rules provide a fix, and the `--fix` flag will apply the fix for all the selected rules that provides an autofix. You can learn more about fixes at https://docs.astral.sh/ruff/linter/#fixes.

Regardless, I don't think this is possible as fix applicability depends on the generated fix itself and is **dynamic** in a way that it could be "safe" or "unsafe" to fix the _same_ rule depending on the source code. And, the `ruff rule` command is meant to provide information about a rule irrespective of the source code. Does that make sense?

---

_Comment by @MichaReiser on 2024-09-09 12:01_

Yes, we can't support this because the fix safety isn't static metadata. It depends on case per case, and the only way to make it static metadata would be to offer fewer fixes, which I don't think is a good outcome.

---

_Closed by @MichaReiser on 2024-09-09 12:01_

---
