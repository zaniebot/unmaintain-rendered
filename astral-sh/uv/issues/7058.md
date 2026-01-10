```yaml
number: 7058
title: "Add a `--output` and `-o` flag to `uv export`"
type: issue
state: closed
author: SamEdwardes
labels:
  - enhancement
  - good first issue
  - cli
assignees: []
created_at: 2024-09-05T01:18:14Z
updated_at: 2024-12-12T14:26:44Z
url: https://github.com/astral-sh/uv/issues/7058
synced_at: 2026-01-10T04:36:20Z
```

# Add a `--output` and `-o` flag to `uv export`

---

_Issue opened by @SamEdwardes on 2024-09-05 01:18_

**Background**

Thank you for adding the `uv export` command. It is a great feature. One improvement that would be helpful is to include an option to export the data to a file. For example, currently you can do this:

```bash
uv export > requirements.txt
```

However, it would be a nice feature to have an output flag. For example:

```bash
uv export --output requirements.txt
# or...
uv export -o requirements.txt
```

This is the same approach that poetry uses: https://python-poetry.org/docs/cli/#export

**Why**

Using `>` or `>>` requires the user to have more advanced shell knowledge. Having an output flag could be easier for users.

**System**

```bash
❯ uv --version
uv 0.4.5 (42b6bfbad 2024-09-04)
```


---

_Label `enhancement` added by @zanieb on 2024-09-05 14:16_

---

_Label `cli` added by @zanieb on 2024-09-05 14:16_

---

_Comment by @zanieb on 2024-09-05 14:16_

We can also do an atomic write with this.

---

_Label `good first issue` added by @zanieb on 2024-09-05 14:17_

---

_Comment by @zanieb on 2024-09-05 21:50_

Also needed for https://github.com/astral-sh/uv-pre-commit/issues/16

---

_Comment by @charliermarsh on 2024-09-05 21:51_

Why is it needed there vs. piping the output? Just curious.

---

_Comment by @zanieb on 2024-09-05 22:00_

I think so users can customize the file name via `args:`? (mostly echoing the chat there)

---

_Comment by @SamEdwardes on 2024-09-06 00:10_

> Why is it needed there vs. piping the output? Just curious.

It is definitely not needed. But I think it is a nice to have:

- Less experience shell users may not be familiar with piping.
- More consistent API across `uv` (e.g. `uv pip compile requirements.in --output-file requirements.txt`) 
  - So maybe using `--output-file` is the best choice to be consistent
- More consistent with other tools:
  - Poetry: `poetry export --output` (https://python-poetry.org/docs/cli/#export)
  - Pip tools: `pip-compile --all-build-deps --all-extras --output-file=constraints.txt` (https://github.com/jazzband/pip-tools)
 - On the other hand... I did some research and some other tools have no export option:
   - conda: https://stackoverflow.com/a/57845418/11349897
   - pipenv: `pipenv requirements > requirements.txt` (https://pipenv.pypa.io/en/latest/advanced.html#generating-a-requirements-txt)

---

_Comment by @charliermarsh on 2024-09-06 00:16_

I will do this quickly.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-06 00:16_

---

_Closed by @charliermarsh on 2024-09-06 00:53_

---

_Closed by @charliermarsh on 2024-09-06 00:53_

---

_Comment by @SamEdwardes on 2024-09-06 15:50_

Awesome thank you Charlie!

---

_Comment by @FishAlchemist on 2024-09-06 18:31_

> Why is it needed there vs. piping the output? Just curious.

I think this feature is necessary, mainly due to text encoding issues.  UV writes files would allow us to unify text encoding. If we use piped output, the encoding would depend on the settings or defaults, just like how PowerShell has different encoding behaviors.

https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_character_encoding?view=powershell-7.4

---

_Comment by @gandalfsaxe on 2024-12-12 14:26_

@charliermarsh Just out of curiosity, why does `uv export` by default only print the result to the terminal, and I need to explicitly add `uv export -o requirements.txt`?

I'm sure there's a good reason for this, it's just that every time I do need to use the `uv export` command, it's because I need the `requirements.txt` file for some legacy systems / pipelines - so I was wondering why the file generation is not just the default behavior?

---
