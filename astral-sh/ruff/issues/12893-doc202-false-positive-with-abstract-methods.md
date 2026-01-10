---
number: 12893
title: DOC202 false positive with abstract methods
type: issue
state: closed
author: MichaReiser
labels: []
assignees: []
created_at: 2024-08-14T14:48:14Z
updated_at: 2024-08-14T14:56:50Z
url: https://github.com/astral-sh/ruff/issues/12893
synced_at: 2026-01-10T01:22:52Z
---

# DOC202 false positive with abstract methods

---

_Issue opened by @MichaReiser on 2024-08-14 14:48_

I really like these new rules, they provide a lot of extra guardrails for docstrings!

When trying to start using rule `DOC202` I ran into a bug. When defining an abstract method, the rule raises an error, because nothing is returned (which makes sense, since it's an empty method, the real code is in the subclasses).

As a small example, an error is raised for this method:

```py
    @property
    @abc.abstractmethod
    def realised_prices(self) -> pd.DataFrame:
        """
        The realised prices of the market.

        Returns:
            The realised prices as a DataFrame
        """
```

_Originally posted by @RubenVanEldik in https://github.com/astral-sh/ruff/issues/12434#issuecomment-2288967263_
            

---

_Comment by @MichaReiser on 2024-08-14 14:53_

This should be resolved by https://github.com/astral-sh/ruff/pull/12771 We plan to get a release out today or tomorrow

---

_Closed by @MichaReiser on 2024-08-14 14:53_

---

_Comment by @RubenVanEldik on 2024-08-14 14:56_

Sorry, I missed the PR that solved this issue, absolutely amazing work!! ❤️❤️❤️ 

---

_Referenced in [astral-sh/ruff#12434](../../astral-sh/ruff/issues/12434.md) on 2024-08-14 14:57_

---

_Referenced in [astral-sh/ruff#18935](../../astral-sh/ruff/issues/18935.md) on 2025-06-25 08:26_

---
