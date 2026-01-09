---
number: 5899
title: Is there a way to tell ruff to ignore comments?
type: issue
state: closed
author: xeon826
labels: []
assignees: []
created_at: 2023-07-19T20:25:30Z
updated_at: 2024-09-24T23:36:48Z
url: https://github.com/astral-sh/ruff/issues/5899
synced_at: 2026-01-07T13:12:15-06:00
---

# Is there a way to tell ruff to ignore comments?

---

_Issue opened by @xeon826 on 2023-07-19 20:25_

Since Ruff is made to work in unison with Black, and Black doesn't reformat comments that exceed the line length, is there a way to tell Ruff to ignore these?


---

_Comment by @zanieb on 2023-07-19 21:18_

@xeon826 if you're using Black to enforce your line length I'd just recommend disabling E501 e.g. https://github.com/PrefectHQ/prefect/blob/a3a2f84ba17b8a4469f29fc2a2bfc6dbffd40ed0/.ruff.toml#L1-L2

---

_Comment by @xeon826 on 2023-07-19 21:39_

I'm using ruff-pre-commit and I added that to its ignore as a workaround but I'm also using ruff as a linting tool and it gives me the diagnostic if comments exceed the line length, I feel like `--ignore-comments` would be a useful feature.

---

_Comment by @zanieb on 2023-07-19 22:54_

Why don't you put it in your Ruff configuration so it is always applied?

Note this is also a duplicate of https://github.com/astral-sh/ruff/issues/4429 and https://github.com/astral-sh/ruff/issues/4595

We [cover this in the FAQ](https://beta.ruff.rs/docs/faq/#is-ruff-compatible-with-black) too.

---

_Comment by @xeon826 on 2023-07-19 23:52_

I use it to check for line length as a linting tool in my environment, if I ignored E501 all together, I wouldn't get that behavior.
![image](https://github.com/astral-sh/ruff/assets/5651215/99af9b21-2295-41cf-9b70-f228fe863850)

pyproject.toml
```
[tool.autoflake]
remove-all-unused-imports = true
in-place = true
expand-star-imports = true
remove-unused-variables = true

[tool.black]
line-length = 88
skip-string-normalization = true
skip-magic-trailing-comma = true
preview = true
target-version = ['py39']

[tool.isort]
profile = 'black'

[tool.ruff]
line-length = 88
target-version = 'py39'
fix = true

[tool.flake8]
max-line-length = 88

[tool.vulture]
exclude = ['venv', 'pod_v3', 'frontend-ops', 'labelbox', 'compute_v2']
paths = ['webapp']
min_confidence = 60
ignore_names = ["visit_*", "do_*"]
sort_by_size = true

```

---

_Comment by @tylerlaprade on 2023-07-23 16:08_

>Black doesn't reformat comments that exceed the line length

@xeon826, are you referring to lines containing _only_ a comment? Black has often rearranged my `# type: ignore` and `# noqa` comments, much to my chagrin, especially because I always include descriptive messages about why I'm ignoring something on that line.

---

_Comment by @tylerlaprade on 2023-07-23 16:12_

As an aside, @xeon826, you can 100% replace the functionality of `autoflake` and `isort` with `ruff`, including all the customization in your `pyproject.toml`. This is 99% likely to be true of `flake8` as well, unless you are enabling some esoteric extension rules outside of your `pyproject.toml`

---

_Comment by @xeon826 on 2023-07-24 19:48_

@tylerlaprade Lines that are only a comment, yes. I'd remove autoflake but ruff doesn't include the `expand-star-imports` feature

---

_Comment by @zanieb on 2023-08-15 03:42_

Since this is a duplicate and there's a workaround I'm going to close this for bookkeeping purposes. Thanks for raising!

---

_Closed by @zanieb on 2023-08-15 03:42_

---

_Comment by @Qinhaifu on 2023-11-17 06:54_

> I'm using ruff-pre-commit and I added that to its ignore as a workaround but I'm also using ruff as a linting tool and it gives me the diagnostic if comments exceed the line length, I feel like `--ignore-comments` would be a useful feature.

i think so too.  `--ignore-comments` would be a useful feature, sometimes we want to write code like this.

```python
demo_code = generate_anything()  # some comments and this line length exceed 79 that i config.
```

---

_Referenced in [rapidsai/cudf#15312](../../rapidsai/cudf/pulls/15312.md) on 2024-03-14 23:48_

---

_Comment by @tapyu on 2024-09-24 23:36_

> --line-length <LINE_LENGTH>  Set the line-length

Don't we have a solution for this yet?

---
