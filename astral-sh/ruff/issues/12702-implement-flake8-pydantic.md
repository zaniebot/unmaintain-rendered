```yaml
number: 12702
title: Implement flake8-pydantic
type: issue
state: open
author: nakashima-hikaru
labels:
  - plugin
  - type-inference
assignees: []
created_at: 2024-08-05T22:51:35Z
updated_at: 2024-10-24T14:33:07Z
url: https://github.com/astral-sh/ruff/issues/12702
synced_at: 2026-01-10T11:09:54Z
```

# Implement flake8-pydantic

---

_Issue opened by @nakashima-hikaru on 2024-08-05 22:51_

Is there any plan to support [flake8-pydantic](https://github.com/Viicos/flake8-pydantic)?
This plugin can be found in pydantic official site.

---

_Label `plugin` added by @charliermarsh on 2024-08-06 02:31_

---

_Renamed from "flake8-pydantic" to "Implement flake8-pydantic" by @dhruvmanila on 2024-08-06 06:23_

---

_Comment by @Viicos on 2024-08-06 11:05_

I will [eventually](https://github.com/Viicos/flake8-pydantic#roadmap) work on this. One drawback I encountered when implementing rules using the AST is class detection. I'm hoping we could leverage the new red knot API at some point.

---

_Comment by @MichaReiser on 2024-08-06 11:10_

It will take quiet some time before the red knot APIs are available in Ruff. 

We generally avoid implementing rules that have a high false negative rate, e.g. because they require multifile analysis because they give user a false sense of safety. That's why I think we shouldn't implement rules requiring multifile analysis to be reasonably accurate today.

---

_Label `multifile-analysis` added by @MichaReiser on 2024-08-06 11:10_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
