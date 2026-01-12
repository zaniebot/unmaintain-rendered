```yaml
number: 673
title: isort incompatibilites with lowercase, MixedCase, UPPERCASE
type: issue
state: closed
author: andersk
labels:
  - isort
assignees: []
created_at: 2022-11-11T00:42:05Z
updated_at: 2022-11-12T03:41:40Z
url: https://github.com/astral-sh/ruff/issues/673
synced_at: 2026-01-12T15:54:40Z
```

# isort incompatibilites with lowercase, MixedCase, UPPERCASE

---

_@andersk_

`isort --profile=black --ca`:

```python
import lowercase
import MixedCase
import UPPERCASE
from module import UPPERCASE, MixedCase, lowercase
```

`ruff --select=I`:

```python
import MixedCase
import UPPERCASE
import lowercase
from module import MixedCase, UPPERCASE, lowercase
```

---

_Comment by @charliermarsh on 2022-11-11 00:57_

Nice, thank you.

---

_Comment by @charliermarsh on 2022-11-11 03:25_

This is described and controlled by the [`order_by_type`](https://pycqa.github.io/isort/docs/configuration/options.html#order-by-type) option.

Do we like this? I don't feel strongly about it.


---

_Comment by @charliermarsh on 2022-11-11 03:26_

To phrase it differently: ignoring isort compatibility, would we want this behavior? (I think it's worth implementing for compatibility purposes.)

---

_Label `isort` added by @charliermarsh on 2022-11-11 03:32_

---

_Comment by @charliermarsh on 2022-11-11 04:16_

Interestingly, it looks like this just applies to `import from` members. For `import` statements, isort sorts in a non-case-sensitive way (which I also find surprising! but it's probably rare to have cased module names anyway?).

---

_Comment by @charliermarsh on 2022-11-11 04:19_

(At the very least, making this clearer in https://github.com/charliermarsh/ruff/pull/676.)

---

_Comment by @andersk on 2022-11-11 05:02_

Yeah, I think compatibility is the main value.

In Zulip, the diff for `combine_as_imports = true` would be +51 −65: fine, if that’s likely to be the future isort default, and it is a bit more compact. The diff for `order_by_type = false` + `case_sensitive = true` would be a further +80 −78: not abhorrent, but would be nicer to avoid, if there’s no particular rationale either way.

I noticed another effect of case-sensitivity:

```diff
-from typing import Type, TypeVar, TypedDict
+from typing import Type, TypedDict, TypeVar
```

which would maybe a weak argument in favor, except that the effect on all-uppercase names is exactly the opposite:

```diff
-from loud_typing import TYPE, TYPED_DICT, TYPE_VAR
+from loud_typing import TYPE, TYPE_VAR, TYPED_DICT
```

🤷

Uppercase modules are indeed rare but do exist: Zulip uses [`DNS`](https://git.launchpad.net/py3dns/tree/README.txt) and [`PIL`](https://pillow.readthedocs.io/en/stable/handbook/tutorial.html).

---

_Comment by @charliermarsh on 2022-11-11 05:20_

Haha yeah, now that I'm being forced to confront it, I find the `isort` behavior pretty confusing. I wish it was all just lexicographic sorting? But like most stylistic issues, I don't have a strong opinion as long as some standard is consistently enforced, and so it makes sense to me to prioritize isort compatibility.


---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-11 18:09_

---

_Closed by @charliermarsh on 2022-11-12 03:41_

---
