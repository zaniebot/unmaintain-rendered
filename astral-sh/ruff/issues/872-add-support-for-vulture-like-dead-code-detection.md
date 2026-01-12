```yaml
number: 872
title: "Add support for `vulture`-like dead code detection"
type: issue
state: open
author: charliermarsh
labels:
  - plugin
  - type-inference
assignees: []
created_at: 2022-11-22T14:27:25Z
updated_at: 2025-09-10T14:37:42Z
url: https://github.com/astral-sh/ruff/issues/872
synced_at: 2026-01-12T15:54:40Z
```

# Add support for `vulture`-like dead code detection

---

_@charliermarsh_

As a maintainer of a large Python codebase, I found [`vulture`](https://github.com/jendrikseipp/vulture) to be really useful, especially for detecting unused functions and unused modules.

This would require us to introduce some new patterns and capabilities that don't yet exist in Ruff, but should! For example, we need to be able to run some analysis over every file (list the used members), then reconcile that analysis post-hoc. We also need to be able to map from references to local modules.


---

_Label `enhancement` added by @charliermarsh on 2022-11-22 14:27_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:18_

---

_Label `plugin` added by @charliermarsh on 2022-12-31 18:18_

---

_Comment by @olliemath on 2023-05-12 12:57_

By the way there is a subset of vulture (and pylint) that would be useful even without mapping out the entire codebase, namely this sort of dead-code detection:

```python
def oops():
    return 1
    return 2  # <- guaranteed never to run
```

---

_Comment by @charliermarsh on 2023-05-12 12:58_

Yeah I'm interested in doing this. @MichaReiser shared some links with me around building these kinds of control-flow graphs. I may write it up as a more detailed issue.

---

_Comment by @saippuakauppias on 2023-06-08 06:48_

+1 we really miss this kind of linter in ruff

The most interesting thing is to find those functions and classes that are forgotten and do not run

---

_Comment by @eronning on 2023-08-30 14:11_

Also +1 on linting these issues in ruff.

---

_Label `multifile-analysis` added by @zanieb on 2023-08-30 15:07_

---

_Comment by @akx on 2023-09-07 13:33_

+1 – would be nice.

Being 100% certain about deadness of names (even locals) can be tricky – e.g. PyCharm generally highlights (well, lowlights) unused locals, _unless_ it notices `vars()`, `locals()`, etc. being used in the same scope, in which case it just throws its paws up and doesn't do that highlighting anymore.

For globals, it gets even harder because a tricky user could be `getattr`ing (or `vars().get`ing) from any given module.

---

_Comment by @FilipeBento on 2023-10-11 16:37_

This would be extremely useful. I understand full implementation requires a lot of work, but checking if methods or variables are unused is very helpful.

---

_Comment by @choweth on 2023-12-19 17:50_

This `vulture`-like tool is great: https://github.com/asottile/dead

---

_Comment by @ofek on 2024-02-20 23:56_

Since this was opened a while ago, have there been any changes internally that would make such an implementation easier?

---

_Comment by @dhruvmanila on 2024-02-21 05:23_

> Since this was opened a while ago, have there been any changes internally that would make such an implementation easier?

Not yet. Adding multi-file analysis is a feature that's on our roadmap which will allow us to support this.

---

_Comment by @alexbondar92 on 2024-08-07 13:04_

also +1 for this feature

---

_Comment by @NeilGirdhar on 2024-09-18 17:26_

If this is implemented, one kind of dead code that is not caught by vulture is when a derived class has a useless override:

```python
class Problem:
    def reward_threshold(self) -> float:
        """The average return threshold at which the problem is considered solved."""
        raise NotImplementedError

class BasicProblem(Problem):  # This class was inserted as part of a refactor.
    @override
    def reward_threshold(self) -> float:
        return 100.0

@dataclass
class GeometryProblem(BasicProblem):
    @override
    def reward_threshold(self) -> float:  # Useless override — this was forgotten during the refactor.
        return 100.0
```

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:32_

---

_Comment by @MedAzizTousli on 2024-10-27 20:49_

Any updates on this?

---

_Comment by @CharlesPerrotMinot on 2025-05-05 05:51_

Can't wait for it, especially with the pitfalls of vulture (doesn't recognize inheritance, or fastapi api tags)!

---

_Comment by @Spenhouet on 2025-09-10 14:37_

Somewhat related feature requests:
- https://github.com/astral-sh/ruff/issues/17724 (also multifile analysis)
- https://github.com/astral-sh/uv/issues/13904 (also on the unused topic)

---
