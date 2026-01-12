```yaml
number: 10906
title: "`RUF100` error despite listed in `per-file-ignores` "
type: issue
state: closed
author: pjonsson
labels:
  - bug
assignees: []
created_at: 2024-04-12T12:44:28Z
updated_at: 2024-04-26T17:19:06Z
url: https://github.com/astral-sh/ruff/issues/10906
synced_at: 2026-01-12T15:54:50Z
```

# `RUF100` error despite listed in `per-file-ignores` 

---

_@pjonsson_

Using Ruff 0.3.7, I tried to select the "RUF" category, and got a `RUF100` error. Adding the file to `per-file-ignores` in pyproject.toml seems to have no effect. The section works for other files/rules in the project, I'm even using it for `RUF005`.

I have not had the RUF category enabled with previous versions, so I do not know if it this is a regression in Ruff. I searched a bit in the issues, not sure if #6385 or #9300 are related to my problem.

Minimizing the file to this:
```python
class Ruf100:
    def to_dict(self):
        return {
            "really_really_long_key": self.really_really_long_designator(),  # noqa
        }
```
and putting it in ruf100.py preserves the error:
```
ruf100.py:4:78: RUF100 [*] Unused blanket `noqa` directive
```
I would be happy to remove the noqa directive, but Flake8 fails on the real code if I remove it.

Here are what I believe the relevant parts of pyproject.toml are, please let me know if you need more:
```toml
[tool.ruff]
src = ["src", "tests"]
target-version = "py310"

[tool.ruff.lint]
select = ["RUF"]
ignore = ["E501"]

[tool.ruff.lint.per-file-ignores]
"src/project/cli.py" = ["UP007", "B008"]

# TO BE FIXED
"src/project/subdir/file.py" = ["RUF015"]
"src/project/otherdir/file2.py" = ["RUF100"]
"ruf100.py" = ["RUF100"]
```


---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-12 13:29_

---

_Comment by @charliermarsh on 2024-04-12 13:29_

I think this is just a bug in Ruff.

---

_Label `bug` added by @charliermarsh on 2024-04-12 13:29_

---

_Comment by @charliermarsh on 2024-04-12 13:29_

By the way, can you add the code to the `# noqa`? Ruff would then work as expected, I think.

---

_Closed by @charliermarsh on 2024-04-12 13:45_

---

_Comment by @pjonsson on 2024-04-12 14:56_

> By the way, can you add the code to the `# noqa`? Ruff would then work as expected, I think.

Changing the noqa to `# noqa: E501` makes Flake8 happy.  Ruff is still unhappy, but about something else:
```
RUF100 [*] Unused `noqa` directive (non-enabled: `E501`)
```
which is correct since I've put E501 in Ruff's ignore category.

I guess that's a policy decision, should Ruff complain about noqa statements that are only used by other linters. Personally, I'm phasing out flake8 and I'm perfectly fine not using the RUF category until the next Ruff release (and then use a blanket noqa which is what is being used currently).

---

_Comment by @pjonsson on 2024-04-20 14:52_

@charliermarsh I just updated to 0.4.1 and still have the same issue, even with `ruf100.py` that I pasted in this ticket.
```
$ poetry run ruff check
ruf100.py:4:78: RUF100 [*] Unused blanket `noqa` directive
$ poetry run ruff --version
ruff 0.4.1
```
I haven't managed to get rid of flake8 yet, so the directive is still in use by flake8.

---

_Comment by @charliermarsh on 2024-04-20 15:15_

Are you sure that your per-file-ignore patterns are defined correctly? Can you confirm that you’re able to ignore other errors in that file?

---

_Comment by @charliermarsh on 2024-04-20 15:20_

Nevermind, I think I understand the case you're hitting. I assume you have no other diagnostics in that file.

---

_Reopened by @charliermarsh on 2024-04-20 15:22_

---

_Closed by @charliermarsh on 2024-04-20 15:33_

---

_Comment by @Avasam on 2024-04-26 17:19_

> Changing the noqa to `# noqa: E501` makes Flake8 happy.  Ruff is still unhappy, but about something else:
> ```
> RUF100 [*] Unused `noqa` directive (non-enabled: `E501`)
> ```
> which is correct since I've put E501 in Ruff's ignore category.

You can set `E501` as "external" since for you it's a Flake8 rule, not a Ruff one. 
https://docs.astral.sh/ruff/settings/#lint_external

---
