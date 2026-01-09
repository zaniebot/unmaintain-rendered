---
number: 1437
title: The requirements.txt file generated does not match pyproject.toml
type: issue
state: closed
author: iphils
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-02-16T05:43:51Z
updated_at: 2024-02-19T18:55:59Z
url: https://github.com/astral-sh/uv/issues/1437
synced_at: 2026-01-07T13:12:16-06:00
---

# The requirements.txt file generated does not match pyproject.toml

---

_Issue opened by @iphils on 2024-02-16 05:43_

I tried to generate a set of locked dependencies from an input of pyproject.toml file so that I could use that to install the dependencies ( as I cannot perform a command like "uv poetry install" )

but the requirements.txt file generated was essentially blank.
Not sure why.
[requirements.txt](https://github.com/astral-sh/uv/files/14306346/requirements.txt)


---

_Comment by @charliermarsh on 2024-02-16 05:44_

Are you able to share the exact command you ran?

---

_Comment by @iphils on 2024-02-16 05:51_

This is the command 
`uv pip compile pyproject.toml -o requirements.txt `

And the toml file is from the below project:
https://github.com/cognitedata/cdf-project-templates

---

_Comment by @charliermarsh on 2024-02-16 06:04_

So I think the issue here is that the `pyproject.toml` for that file uses Poetry's custom metadata syntax for its dependencies, but `uv` only supports reading the standardized metadata under `[project.dependencies]`. You can see an example of what that looks like here: https://github.com/python-trio/trio/blob/ea8f4be2454f7aaf984852f60ff7ce2b966e695e/pyproject.toml#L38.

---

_Label `question` added by @MichaReiser on 2024-02-16 07:37_

---

_Comment by @EnriqueSoria on 2024-02-16 08:55_

> So I think the issue here is that the `pyproject.toml` for that file uses Poetry's custom metadata syntax for its dependencies, but `uv` only supports reading the standardized metadata under `[project.dependencies]`. You can see an example of what that looks like here: https://github.com/python-trio/trio/blob/ea8f4be2454f7aaf984852f60ff7ce2b966e695e/pyproject.toml#L38.

Is adding support for poetry [dependency groups](https://python-poetry.org/docs/managing-dependencies/#dependency-groups) (or something similar) within your plans? 

---

_Comment by @hauntsaninja on 2024-02-19 18:52_

Duplicate of #1619 , #1630 and others 

---

_Comment by @AlexWaygood on 2024-02-19 18:55_

Yep, folding into #1630

---

_Closed by @AlexWaygood on 2024-02-19 18:55_

---

_Label `question` removed by @AlexWaygood on 2024-02-19 18:55_

---

_Label `duplicate` added by @AlexWaygood on 2024-02-19 18:55_

---

_Label `question` added by @AlexWaygood on 2024-02-19 18:55_

---
