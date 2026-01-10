```yaml
number: 705
title: "Make `combine_as_imports = false` the default sorting behavior"
type: issue
state: closed
author: charliermarsh
labels:
  - isort
assignees: []
created_at: 2022-11-12T17:32:53Z
updated_at: 2022-12-04T13:02:58Z
url: https://github.com/astral-sh/ruff/issues/705
synced_at: 2026-01-10T12:09:58Z
```

# Make `combine_as_imports = false` the default sorting behavior

---

_Issue opened by @charliermarsh on 2022-11-12 17:32_

This would make our import sorting drop-in compatible with `isort`, but it's a bit more complicated, I don't prefer the behavior, and there are a variety of comments on the `isort` repo suggesting they wanted to make `true` the default anyway.

https://pycqa.github.io/isort/docs/configuration/options.html#combine-as-imports


---

_Label `question` added by @charliermarsh on 2022-11-12 17:32_

---

_Comment by @tiangolo on 2022-11-13 18:23_

I just came to ask for this... I know I said I didn't have a preference, but adding Ruff to FastAPI I just realized I do. 😅

Most of the "import as" I have are just to re-export things from other places, so that mypy and PyRight can know they are available, without having to deal with `__all__` that is much more error prone (strings referring to code can get obsolete and out of sync easily).

And having those "import as" in their own line helps me see more explicitly what are the things that I'm re-exporting and don't get lost among the things I'm just importing to use internally.

---

_Comment by @charliermarsh on 2022-11-13 18:33_

Yeah makes sense! Will change this. (There’s maybe a case to be made that “from a import b as b” and “from a import b as c” should get different treatment, but probably worth just being drop-in compatible with isort at this point.)


---

_Renamed from "Should we make `combine_as_imports = false` the default sorting behavior?" to "Make `combine_as_imports = false` the default sorting behavior" by @charliermarsh on 2022-11-13 18:52_

---

_Label `question` removed by @charliermarsh on 2022-11-13 18:53_

---

_Label `isort` added by @charliermarsh on 2022-11-13 18:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-13 18:53_

---

_Closed by @charliermarsh on 2022-11-14 04:07_

---

_Comment by @Jackenmen on 2022-12-04 01:53_

> This would make our import sorting drop-in compatible with `isort`, but it's a bit more complicated, I don't prefer the behavior, and there are a variety of comments on the `isort` repo suggesting they wanted to make `true` the default anyway.

@charliermarsh is there an option for combining `as` imports available? It doesn't seem to based on the documentation and the PR that closed this issue but I'm finding that surprising considering that you said you prefer it :thinking:
For what it's worth, I also prefer it and it's the only discrepancy from the default `black` profile in all projects where I have isort set up.

---

_Comment by @charliermarsh on 2022-12-04 02:16_

@Jackenmen - Yeah I ended up changing the default behavior and didn't think it was worth making it configurable, but I can probably support it. Will take a look...


---

_Comment by @charliermarsh on 2022-12-04 04:12_

@Jackenmen - I've implemented it here: https://github.com/charliermarsh/ruff/pull/1022. Will go out shortly. Can you give it a try and let me know if it matches your expectation?

---

_Comment by @Jackenmen on 2022-12-04 13:02_

It appears to work as I expect, checked first on one made-up example and then on two isorted codebases, thanks!

---
