```yaml
number: 4754
title: Writing lint output to file
type: issue
state: closed
author: TalAmuyal
labels:
  - configuration
assignees: []
created_at: 2023-05-31T13:37:55Z
updated_at: 2023-06-20T16:46:50Z
url: https://github.com/astral-sh/ruff/issues/4754
synced_at: 2026-01-12T15:54:44Z
```

# Writing lint output to file

---

_@TalAmuyal_

**TL;DR:** I couldn't find how to tell Ruff to write the report to file.

**Examples:**
- `bandit` -> `--output=reports/report.txt`
- `flake8` -> `--output-file=reports/report.txt`
- `pylint` -> `--output-format=text:reports/report.txt`

**Motivation:**

I'm using Ruff with [Pants](https://www.pantsbuild.org/docs/python-linters-and-formatters).
Pants parallelises Ruff's run for monorepos.
In general, it runs linters in a sandbox, collects their reports and puts them in `dist/[linter]/...` which can then be used by Jenkins to display the results to the user.

---

_Comment by @charliermarsh on 2023-05-31 15:22_

Naive question, but what's the difference between an `--output-file` argument and `ruff > report.txt`?

---

_Label `question` added by @charliermarsh on 2023-05-31 15:22_

---

_Label `configuration` added by @charliermarsh on 2023-05-31 15:22_

---

_Comment by @TalAmuyal on 2023-05-31 15:28_

The issue with that is it requires to add a shell (e.g bash) between `ruff` and the process that invokes it.

Currently, I'm running Pants which runs Ruff, rather than running a Bash script or Make that then runs Ruff.

---

_Comment by @TalAmuyal on 2023-05-31 15:48_

To be clear, I really appreciate your work and the fast response, and I submit this as a question or a feature-request.
If the above is perceived as offensive in any way, please know that it is unintentional and that I'm extremely grateful and do not take this work or your time for granted.

---

_Comment by @charliermarsh on 2023-05-31 18:28_

No worries at all, I appreciate the kind words, and I didn't take it as offensive in any way -- it's a super reasonable request! I just wanted to understand the context to make sure I could probably evaluate whether it's a worth supporting as a new CLI option. It does seem reasonable to me -- any reactions, @MichaReiser?

---

_Comment by @JonathanPlasse on 2023-05-31 19:01_

Related
- #995
- #2388

---

_Comment by @TalAmuyal on 2023-06-01 15:24_

In `pants.toml`, the following needs to be added for Pylint:
```toml
# pants.toml

[pylint]
args = ["--output-format=text:reports/report.txt"]
```

Then, Pants adds `--output-format=text:reports/report.txt` as an argument to Pylint when it is executed.
After Pylint completes its execution, Pants collects the files under `reports/` (only `report.txt` in this example) and puts them in `dist/pylint/` (or under `dist/flake8/` etc.).

I imagine that for Ruff it will be something like:
```toml
# pants.toml

[ruff]
args = ["--format=junit", "--report-path=reports/report.xml"]
```

Does this answer your question?

---

_Comment by @TalAmuyal on 2023-06-01 15:48_

Should I close this issue and sum it up in https://github.com/charliermarsh/ruff/issues/995?

---

_Comment by @MichaReiser on 2023-06-05 07:08_

Seems reasonable. `rustc` provides the `-o` option

>  -o FILENAME         Write output to <filename>

ESLint:

```
Output:
  -o, --output-file path::String  Specify file to write report to
  -f, --format String             Use a specific output format - default: stylish
  --color, --no-color             Force enabling/disabling of color
```




---

_Label `question` removed by @charliermarsh on 2023-06-05 19:21_

---

_Comment by @dhruvmanila on 2023-06-06 14:07_

Yes, I think `-o`/`--output-file` flags are reasonable. I can take this up.

Some observations about what `eslint` does in various scenarios:
1. `eslint --fix --format=junit --output-file=eslint.out .` - This will write the fixed content back to the original files and remaining diagnostics will be written to the output file in the specified format.
2. `eslint --fix-dry-run --format=junit --output-file=eslint.out .` - The dry run doesn't produce any output so the only output is the remaining diagnostics _if_ the fixes were applied.
3. `eslint --format=stylish --output-file=eslint.out .` - This will write the stylish output in the file which includes the [ASCII escape sequences](https://en.wikipedia.org/wiki/ANSI_escape_code) as well.

Eslint won't output anything if the `--output-file` option is provided.

For Ruff, I think we should default to `NO_COLOR=1` in case we're writing the output to a file avoid creating confusion when that file is viewed/parsed.

---

_Assigned to @dhruvmanila by @charliermarsh on 2023-06-06 14:28_

---

_Closed by @dhruvmanila on 2023-06-20 16:46_

---
