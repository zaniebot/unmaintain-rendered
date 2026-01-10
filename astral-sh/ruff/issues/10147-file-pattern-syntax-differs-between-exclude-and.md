---
number: 10147
title: File pattern syntax differs between exclude and per-file-ignores
type: issue
state: open
author: neelmraman
labels:
  - configuration
  - needs-design
assignees: []
created_at: 2024-02-27T19:52:07Z
updated_at: 2024-06-07T04:03:17Z
url: https://github.com/astral-sh/ruff/issues/10147
synced_at: 2026-01-10T01:22:49Z
---

# File pattern syntax differs between exclude and per-file-ignores

---

_Issue opened by @neelmraman on 2024-02-27 19:52_

Hi, thanks for the work on this great project! One thing I found kind of surprising (and that doesn't seem to be explicitly mentioned in the docs anywhere) is that `exclude`/`extend-exclude` and `per-file-ignores`/`extend-per-file-ignores` seem to use a different syntax for specifying file patterns. 

Here is a fairly minimal way to reproduce on ruff v0.2.2:
1. create a new repo with this structure
```
.
├── bar_dir
│   └── a.py
├── foo_dir
│   └── a.py
└── pyproject.toml
```
where `pyproject.toml` is
```toml
[tool.ruff]
exclude = ["foo_dir"]

[tool.ruff.lint]
select = ["F"]

[tool.ruff.lint.per-file-ignores]
"bar_dir" = ["F401"]
```
and both directories have an `a.py` that looks like
```python
import os

print("hello")
```
2. run `ruff check`, and you should see an error reported for the bar_dir file but not the foo_dir file, implying the directory name alone worked as a pattern in `exclude` but not in `per-file-ignores`
3. update `pyproject.toml` so that the `per-file-ignores` is now something like `"bar_dir/**/*.py" = ["F401"]`, and run `ruff check` again, which should produce no errors.

---

_Label `configuration` added by @AlexWaygood on 2024-02-27 21:35_

---

_Comment by @MichaReiser on 2024-03-04 12:45_

Agree that this is confusing and something I ran into myself with [`lint.exclude` and `format.exclude`](https://github.com/astral-sh/ruff/issues/8267). We should revisit the glob patterns in our configuration

---

_Label `needs-design` added by @zanieb on 2024-03-11 17:36_

---

_Comment by @zanieb on 2024-03-11 17:36_

If anyone has the time, a comparison of the different formats with some notes would be helpful for designing a path forward here.

---

_Comment by @Boon-in-Oz on 2024-06-05 02:47_

I've run into this problem too. I cannot come up with a pattern to match a directory in per-file-ignores. I can ignore a file by name, but I can't ignore all files in a directory. Does anyone have a workaround? That is, a way to exclude a directory?

I've tried all of these without success:
```
"**/dir_name/*.py"
"dir_name/**.py"
"/dir_name/**.py"
"dir_name"
"dir_name/*"
"dir_name/**"
```

---

_Comment by @charliermarsh on 2024-06-05 16:42_

@Boon-in-Oz -- Can you share a full reproducible example? `"**/dir_name/*.py"` works for me as expected.

Specifically, I created `bar/baz/bop.py` with an unused import, and the following `pyproject.toml` in the current working directory:

```toml
[tool.ruff.lint.per-file-ignores]
"**/baz/*.py" = ["F401"]
```

`ruff check .` shows no error.


---

_Comment by @Boon-in-Oz on 2024-06-06 08:26_

Hey thanks for the quick response. You're correct, that does work. 

I think it's due to my using the "extend" command in ruff.toml. I can reproduce it like this:

I'm working in a multi-root workspace in VS Code, using Ruff for linting, rather than running Ruff from the command line.
I have two ruff.toml files. One is in the same dir as foo_dir and bar_dir in the example. The other is in a different root entirely, which seems to be important.

So in my case my code-workspace file is
```
{
	"folders": [
        {
            "name": "base",
            "path": "."
        },
        {
            "name": "more",
            "path": "../../parts"
        },
    ],
}
```
With foo_dir and bar_dir both in the "base" directory. In bar_dir there is an a.py that looks like
```
fooBar = 1
```
ruff.toml in the "base" directory looks like
```
extend = "../../../parts/ruff.toml"
```
and parts/ruff.toml looks like
```
[lint]
select = ["N"]

[lint.per-file-ignores]
"**/bar_dir/*.py" = ["N816"]
#"a.py" = ["N816"]
```
bar_dir/a.py shows error N816.
If I comment that last line in the second ruff.toml, the error goes away. If I move the [lint.per-file-ignores] section to the first ruff.toml, the error goes away.

(Sorry I hope that makes sense. I'm new to reporting Git issues and out of time for today but I can try to upload this all tomorrow. Also, I think I have my workaround: I can move the ignores to the other ruff.toml easily enough.)

---

_Comment by @charliermarsh on 2024-06-07 03:11_

That actually still works for me, emulating the `extend`.

You can try running `ruff check --show-settings` in the directory to see what the resolved `per-file-ignore` patterns look like.

---

_Comment by @Boon-in-Oz on 2024-06-07 03:42_

Thanks. So running this in my "base" dir (C:\test\test\ruff_test)
```
"path\to\ruff.exe" check --show-settings bar_dir\a.py
```
Gives this for the `linter.per_file_ignores` section:
```
linter.per_file_ignores = {
        absolute_matcher = "C:\test\parts\**\bar_dir\*.py"
basename_matcher = "**/bar_dir/*.py"
negated = false
rules = [
        mixed-case-variable-in-global-scope (N816),
]

}
```
which matches the result I see. The path `"**/bar_dir/*.py"` is resolved under the local directory of `C:\test\parts\ruff.toml`, resulting in `"C:\test\parts\**\bar_dir\*.py"`, so it doesn't match `"C:\test\test\ruff_test\bar_dir\a.py"`

---

_Comment by @charliermarsh on 2024-06-07 03:48_

What is the `extends` exactly in this case? And what is the `trees\cod\trunk` path?

---

_Comment by @Boon-in-Oz on 2024-06-07 04:01_

bar_dir/ruff.toml contains only
`extend = "../../../parts/ruff.toml"`
And whoops I didn't consistently modify all my paths when I typed them. That should be `C:\test`. Fixed now.

Here are the files you need for my repro:
[test.zip](https://github.com/user-attachments/files/15704557/test.zip)

I unzipped into C:, navigated (in a Windows cmd prompt) to C:\test, then ran
`ruff.exe check --no-fix test\ruff_test\bar_dir\a.py`
and got the N816 error.

---
