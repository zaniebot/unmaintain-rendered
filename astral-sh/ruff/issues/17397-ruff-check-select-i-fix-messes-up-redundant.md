```yaml
number: 17397
title: "`ruff check --select I --fix` messes up redundant import aliases"
type: issue
state: closed
author: pekkaklarck
labels: []
assignees: []
created_at: 2025-04-14T19:40:45Z
updated_at: 2025-04-15T01:37:17Z
url: https://github.com/astral-sh/ruff/issues/17397
synced_at: 2026-01-10T11:09:58Z
```

# `ruff check --select I --fix` messes up redundant import aliases

---

_Issue opened by @pekkaklarck on 2025-04-14 19:40_

### Summary

We have started to use redundant import aliases like `from mod import Class as Class` when defining public module and package APIs. For example, [this module](https://github.com/robotframework/robotframework/blob/master/src/robot/api/parsing.py#L488) has code like this:

```python
from robot.parsing import (
    get_tokens as get_tokens,
    get_resource_tokens as get_resource_tokens,
    get_init_tokens as get_init_tokens,
    get_model as get_model,
    get_resource_model as get_resource_model,
    get_init_model as get_init_model,
    Token as Token,
)
from robot.parsing.model.blocks import (
    File as File,
    SettingSection as SettingSection,
    VariableSection as VariableSection,
    TestCaseSection as TestCaseSection,
    KeywordSection as KeywordSection,
    CommentSection as CommentSection,
    TestCase as TestCase,
    Keyword as Keyword,
    If as If,
    Try as Try,
    For as For,
    While as While,
    Group as Group,
)
```

We are experimenting with automatic code formatting, including automatically sorting imports. My understanding is that with Ruff the command to use if `ruff check --select I --fix`. When I use that command to format the above code, I get this:
```python
from robot.parsing import (
    Token as Token,
)
from robot.parsing import (
    get_init_model as get_init_model,
)
from robot.parsing import (
    get_init_tokens as get_init_tokens,
)
from robot.parsing import (
    get_model as get_model,
)
from robot.parsing import (
    get_resource_model as get_resource_model,
)
from robot.parsing import (
    get_resource_tokens as get_resource_tokens,
)
from robot.parsing import (
    get_tokens as get_tokens,
)
from robot.parsing.model.blocks import (
    CommentSection as CommentSection,
)
from robot.parsing.model.blocks import (
    File as File,
)
from robot.parsing.model.blocks import (
    For as For,
)
from robot.parsing.model.blocks import (
    Group as Group,
)
from robot.parsing.model.blocks import (
    If as If,
)
from robot.parsing.model.blocks import (
    Keyword as Keyword,
)
from robot.parsing.model.blocks import (
    KeywordSection as KeywordSection,
)
from robot.parsing.model.blocks import (
    SettingSection as SettingSection,
)
from robot.parsing.model.blocks import (
    TestCase as TestCase,
)
from robot.parsing.model.blocks import (
    TestCaseSection as TestCaseSection,
)
from robot.parsing.model.blocks import (
    Try as Try,
)
from robot.parsing.model.blocks import (
    VariableSection as VariableSection,
)
from robot.parsing.model.blocks import (
    While as While,
)
```

Although imports now are sorted, I consider the end result horrible. I consider this a bug, but if this is intentional, I propose a style change. In my opinion the original format is good except for ordering.


### Version

ruff 0.11.5

---

_Comment by @pekkaklarck on 2025-04-14 19:44_

As a reference, below is the code that isort produces with default settings. I don't like that either, but it isn't as bad as the above.

```python
from robot.parsing import Token as Token
from robot.parsing import get_init_model as get_init_model
from robot.parsing import get_init_tokens as get_init_tokens
from robot.parsing import get_model as get_model
from robot.parsing import get_resource_model as get_resource_model
from robot.parsing import get_resource_tokens as get_resource_tokens
from robot.parsing import get_tokens as get_tokens
from robot.parsing.model.blocks import CommentSection as CommentSection
from robot.parsing.model.blocks import File as File
from robot.parsing.model.blocks import For as For
from robot.parsing.model.blocks import Group as Group
from robot.parsing.model.blocks import If as If
from robot.parsing.model.blocks import Keyword as Keyword
from robot.parsing.model.blocks import KeywordSection as KeywordSection
from robot.parsing.model.blocks import SettingSection as SettingSection
from robot.parsing.model.blocks import TestCase as TestCase
from robot.parsing.model.blocks import TestCaseSection as TestCaseSection
from robot.parsing.model.blocks import Try as Try
from robot.parsing.model.blocks import VariableSection as VariableSection
from robot.parsing.model.blocks import While as While
```

---

_Comment by @ntBre on 2025-04-14 20:23_

This looks like a duplicate of #16399. As stated in this comment (https://github.com/astral-sh/ruff/issues/16399#issuecomment-2685560342):

> It is due to the last item ending with a trailing comma (see https://docs.astral.sh/ruff/settings/#lint_isort_split-on-trailing-comma). You get the isort formatting if you remove the trailing comma.

I tested this on your example, and I also get the desired isort output if I remove the trailing commas from both of the initial blocks. The other issue is still labeled as a bug, though, so this is something we want to fix!

---

_Closed by @ntBre on 2025-04-14 20:23_

---

_Comment by @pekkaklarck on 2025-04-14 20:58_

Yeah, this is dupe. I tried searching for similar issues but failed with generic terms like "from import as".

Although the isort default format is much better than what Ruff now produces, it don't consider it "desired". I'd much rather see Ruff producing the same format as the original and not splitting as-imports. Notice that isort makes this behavior [configurable](https://pycqa.github.io/isort/docs/configuration/options.html#combine-as-imports) and apparently the [default will change in the next major release](https://github.com/PyCQA/isort/issues/1305#issuecomment-926369825). Using just `--combine-as` with isort doesn't yield good results, though, but `isort --combine-as --multi-line=5 --split-on-trailing-comma` produces output that's Black and Ruff compatible:

```python
from robot.parsing import (
    Token as Token,
    get_init_model as get_init_model,
    get_init_tokens as get_init_tokens,
    get_model as get_model,
    get_resource_model as get_resource_model,
    get_resource_tokens as get_resource_tokens,
    get_tokens as get_tokens,
)
from robot.parsing.model.blocks import (
    CommentSection as CommentSection,
    File as File,
    For as For,
    Group as Group,
    If as If,
    Keyword as Keyword,
    KeywordSection as KeywordSection,
    SettingSection as SettingSection,
    TestCase as TestCase,
    TestCaseSection as TestCaseSection,
    Try as Try,
    VariableSection as VariableSection,
    While as While,
)
```
I believe the above should be the target for Ruff's import sorting. If you are open to this idea, I can submit a separate issue about the format change where this can be discussed more.


---

_Comment by @ntBre on 2025-04-14 21:22_

Oh, we also support isort's [`combine-as-imports`](https://docs.astral.sh/ruff/settings/#lint_isort_combine-as-imports) setting. If you enable that even with the trailing commas, we only sort your initial example within its existing sections:

```shell
ruff check --select I --diff --config 'lint.isort.combine-as-imports=true'
```

Output:

```diff
--- fmt.py
+++ fmt.py
@@ -1,24 +1,24 @@
 from robot.parsing import (
-    get_tokens as get_tokens,
-    get_resource_tokens as get_resource_tokens,
+    Token as Token,
+    get_init_model as get_init_model,
     get_init_tokens as get_init_tokens,
     get_model as get_model,
     get_resource_model as get_resource_model,
-    get_init_model as get_init_model,
-    Token as Token,
+    get_resource_tokens as get_resource_tokens,
+    get_tokens as get_tokens,
 )
 from robot.parsing.model.blocks import (
+    CommentSection as CommentSection,
     File as File,
+    For as For,
+    Group as Group,
+    If as If,
+    Keyword as Keyword,
+    KeywordSection as KeywordSection,
     SettingSection as SettingSection,
-    VariableSection as VariableSection,
+    TestCase as TestCase,
     TestCaseSection as TestCaseSection,
-    KeywordSection as KeywordSection,
-    CommentSection as CommentSection,
-    TestCase as TestCase,
-    Keyword as Keyword,
-    If as If,
     Try as Try,
-    For as For,
+    VariableSection as VariableSection,
     While as While,
-    Group as Group,
 )

Would fix 1 error.
```

Is that the behavior you'd expect? Maybe I should comment this on the original issue too, if so.

---

_Comment by @pekkaklarck on 2025-04-14 21:47_

That looks excellent! I actually didn't know that `ruff check --fix` supports configuration, I thought it's "opinionated" like other formatters seem to be. Happy to be wrong! I'm all for sensible defaults, but often _some_ configuration helps.

Talking about sensible defaults, I still believe the defaults for as-import handling should be changed. It's odd that
```python
from module import (
    First,
    Second,
)
```
is accepted as-is, but
```python
from module import (
    First as First,
    Second as Second,
)
```
is not.

---

_Comment by @ntBre on 2025-04-15 01:25_

The formatter is pretty opinionated, but the `isort` rules are somewhat counter-intuitively in the linter, which is more configurable.

I think you may be right about the default value, especially if `isort` is changing theirs. I'll summarize this discussion in the other issue (unless you'd like to), and then maybe we can discuss the default there too.

Thanks again for the report and the `isort` links!

---
