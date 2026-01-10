```yaml
number: 6546
title: "docs: made `uvx` note more explict"
type: pull_request
state: merged
author: ZeyadMoustafaKamal
labels:
  - documentation
assignees: []
merged: true
base: main
head: improve_docs
created_at: 2024-08-23T20:48:41Z
updated_at: 2024-08-23T22:17:38Z
url: https://github.com/astral-sh/uv/pull/6546
synced_at: 2026-01-10T13:09:51Z
```

# docs: made `uvx` note more explict

---

_Pull request opened by @ZeyadMoustafaKamal on 2024-08-23 20:48_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I used `uvx` to test my code using `pytest` it was just before the documentation and it worked pretty fine. But when I saw the docs I was confused as it says:

> If you are running a tool in a project and the tool requires that your project is installed, e.g., when using `pytest` or `mypy`, you'll want to use `uv` run instead of `uvx`. Otherwise, the tool will be run in a virtual environment that is isolated from your project.

So to make it simple if you don't recommend using `uvx` in this situation then here is the pull request, and if not just close this pull request. I said that I don't have to open an issue to discuss this as it's so simple.

## Test Plan

None


---

_@zanieb reviewed on 2024-08-23 20:55_

---

_Review comment by @zanieb on `docs/guides/tools.md`:49 on 2024-08-23 20:55_

Does `pytest` actually work here? You're in a project and running `uvx pytest` and your tests import your project? Are you using a flat project structure?

---

_Comment by @zanieb on 2024-08-23 20:55_

Thanks for sharing! I'm a bit confused but it sounds like we are missing something here.

---

_@ZeyadMoustafaKamal reviewed on 2024-08-23 21:00_

---

_Review comment by @ZeyadMoustafaKamal on `docs/guides/tools.md`:49 on 2024-08-23 21:00_

well [here is the project I used](https://github.com/ZeyadMoustafaKamal/django-rest-admin) clone it and see if this is the same thing you were talking about.

---

_@zanieb reviewed on 2024-08-23 21:01_

---

_Review comment by @zanieb on `docs/guides/tools.md`:49 on 2024-08-23 21:01_

Yeah this is because you're using a flat project structure, so the project can be imported _without_ being installed (vs e.g., using `src/<module>` for your code)

---

_@zanieb reviewed on 2024-08-23 21:01_

---

_Review comment by @zanieb on `docs/guides/tools.md`:49 on 2024-08-23 21:01_

I can try to improve the docs here.

---

_@zanieb reviewed on 2024-08-23 21:02_

---

_Review comment by @zanieb on `docs/guides/tools.md`:47 on 2024-08-23 21:02_

"e.g., when using `pytest` or `mypy` with your code in a `src/` directory" 

---

_@ZeyadMoustafaKamal reviewed on 2024-08-23 21:03_

---

_Review comment by @ZeyadMoustafaKamal on `docs/guides/tools.md`:49 on 2024-08-23 21:03_

fine. and thanks a lot for your hard work.

---

_@ZeyadMoustafaKamal reviewed on 2024-08-23 21:03_

---

_Review comment by @ZeyadMoustafaKamal on `docs/guides/tools.md`:49 on 2024-08-23 21:03_

Also I think we can close this now or what you think

---

_@ZeyadMoustafaKamal reviewed on 2024-08-23 21:05_

---

_Review comment by @ZeyadMoustafaKamal on `docs/guides/tools.md`:47 on 2024-08-23 21:05_

I think we should use the term 'flat project' instead of saying the `src` directory. If someone didn't know what it means like me they can search for it.

---

_@zanieb reviewed on 2024-08-23 21:09_

---

_Review comment by @zanieb on `docs/guides/tools.md`:47 on 2024-08-23 21:09_

Okay, that's fine with me — want to make that change here and we can merge your pull request?

---

_@ZeyadMoustafaKamal reviewed on 2024-08-23 21:12_

---

_Review comment by @ZeyadMoustafaKamal on `docs/guides/tools.md`:47 on 2024-08-23 21:12_

fine.

---

_Label `documentation` added by @zanieb on 2024-08-23 21:13_

---

_@ZeyadMoustafaKamal reviewed on 2024-08-23 21:20_

---

_Review comment by @ZeyadMoustafaKamal on `docs/guides/tools.md`:47 on 2024-08-23 21:20_

done

---

_Merged by @zanieb on 2024-08-23 22:17_

---

_Closed by @zanieb on 2024-08-23 22:17_

---
