---
number: 10519
title: "Folder with module name detected as source leads to false `I001` error"
type: issue
state: open
author: carschno
labels:
  - isort
  - "priority:medium"
assignees: []
created_at: 2024-03-22T08:22:28Z
updated_at: 2025-11-29T13:39:38Z
url: https://github.com/astral-sh/ruff/issues/10519
synced_at: 2026-01-07T13:12:15-06:00
---

# Folder with module name detected as source leads to false `I001` error

---

_Issue opened by @carschno on 2024-03-22 08:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
This issue affects the expected sorting of imports in Python and hence (incorrectly) triggers a `I001` error.

When there is a folder that has the same name as a module, it is (possibly incorrectly) identified as `SourceMatch`. Ruff then categorizes the module as `Known(FirstParty)` and adapts the expected sorting accordingly.

This happens commonly, but not exclusively, when using the [wandb library](https://docs.wandb.ai/ref/python/) because it creates a `wandb` folder, as discovered in https://github.com/ChartBoost/ruff-action/issues/20.

To reproduce the issue, create a Python file with these imports (I called it `test.py`):

```python
import csv
import logging
import random
import sys
from typing import Any, Optional, TextIO

import torch
import torch.nn as nn
from torch import optim
from torcheval.metrics import (
    Metric,
    MulticlassAccuracy,
    MulticlassF1Score,
    MulticlassPrecision,
    MulticlassRecall,
)
from tqdm import tqdm

import wandb
```
In the initial scenario, there is a `wandb` directory:
```console
% ls -d wandb/
wandb/
```
Running Ruff to check the import sorting:
```console
% poetry run ruff check -v --select=I001 test.py                                            
[2024-03-22][08:57:28][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/carstenschnober/LAHTeR/workspace/document-segmentation/pyproject.toml
[2024-03-22][08:57:28][ruff::commands::check][DEBUG] Identified files to lint in: 2.013375ms
[2024-03-22][08:57:28][ruff::diagnostics][DEBUG] Checking: /Users/carstenschnober/LAHTeR/workspace/document-segmentation/test.py
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'torch.nn' as Known(ThirdParty) (NoMatch)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'csv' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'random' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'sys' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'logging' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'torch' as Known(ThirdParty) (NoMatch)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'wandb' as Known(FirstParty) (SourceMatch("/Users/carstenschnober/LAHTeR/workspace/document-segmentation"))
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'torch' as Known(ThirdParty) (NoMatch)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'torcheval.metrics' as Known(ThirdParty) (NoMatch)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'typing' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][08:57:28][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'tqdm' as Known(ThirdParty) (NoMatch)
[2024-03-22][08:57:28][ruff::commands::check][DEBUG] Checked 1 files in: 765.709µs
All checks passed!
```
The checks pass, `wandb` has been categorized as `Known(FirstParty)`

Now remove the `wandb` directory:
```console
% mv wandb wandb.bak
% ls -d wandb/
ls: wandb/: No such file or directory
```
Running the same Ruff check triggers a `I001` error on the same file, categorizing `wandb` as `Known(ThirdParty)`; the module categorization is cached, so I remove the `.ruff_cache` directory first to reproduce the error:

```console
% rm -r .ruff_cache                             
% poetry run ruff check -v --select=I001 test.py
[2024-03-22][09:06:42][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/carstenschnober/LAHTeR/workspace/document-segmentation/pyproject.toml
[2024-03-22][09:06:42][ruff::commands::check][DEBUG] Identified files to lint in: 1.959083ms
[2024-03-22][09:06:42][ruff::diagnostics][DEBUG] Checking: /Users/carstenschnober/LAHTeR/workspace/document-segmentation/test.py
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'torch.nn' as Known(ThirdParty) (NoMatch)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'csv' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'random' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'sys' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'logging' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'torch' as Known(ThirdParty) (NoMatch)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'wandb' as Known(FirstParty) (SourceMatch("/Users/carstenschnober/LAHTeR/workspace/document-segmentation"))
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'torch' as Known(ThirdParty) (NoMatch)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'torcheval.metrics' as Known(ThirdParty) (NoMatch)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'typing' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-03-22][09:06:42][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'tqdm' as Known(ThirdParty) (NoMatch)
[2024-03-22][09:06:42][ruff::commands::check][DEBUG] Checked 1 files in: 1.004834ms
test.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 fixable with the `--fix` option.
```
This is now the expected sorting that is generated when calling `ruff --fix` call above:
```python
import csv
import logging
import random
import sys
from typing import Any, Optional, TextIO

import torch
import torch.nn as nn
import wandb
from torch import optim
from torcheval.metrics import (
    Metric,
    MulticlassAccuracy,
    MulticlassF1Score,
    MulticlassPrecision,
    MulticlassRecall,
)
from tqdm import tqdm
```
This configuration option fixes the issue properly (see https://github.com/ChartBoost/ruff-action/issues/20#issuecomment-2012747916):
```toml
[tool.ruff.lint.isort]
known-third-party = ["wandb"]
```

However, it is difficult for users to identify the issue and fix the configuration accordingly. I think a better solution would be to have a more robust source directory detection.
A heuristics like checking for `__init__.py` or generally the presence of `*.py` files as a condition might be a solid starting point for Python.



---

_Referenced in [ChartBoost/ruff-action#20](../../ChartBoost/ruff-action/issues/20.md) on 2024-03-22 08:23_

---

_Renamed from "Folder with module name detected as source" to "Folder with module name detected as source leads to false `I001` error" by @carschno on 2024-03-24 11:01_

---

_Comment by @charliermarsh on 2024-03-24 20:00_

We could consider requiring an `__init__.py` but it would be a breaking change, I think.

---

_Label `isort` added by @charliermarsh on 2024-03-24 20:00_

---

_Comment by @carschno on 2024-03-25 08:21_

> We could consider requiring an `__init__.py` but it would be a breaking change, I think.

I see how a folder that is now correctly categorized as a source folder might not have an `__init__.py` in some cases, so this requirement could perhaps be something for the next major release indeed.

If a source folder is required to contain any `*.py` file, however, I cannot see how it would break current behaviour -- except for cases in which the sorting has now been incorrectly adapted to identified source folders.
So perhaps requiring any `.py` file could be an intermediate fix, whereas the next major release requires an `__init__.py`?

If somebody could point me to the relevant source code, I could try and work on a PR.


---

_Comment by @bkad on 2024-04-04 00:37_

>Running the same Ruff check triggers a I001 error on the same file, categorizing wandb as Known(ThirdParty); the module categorization is cached, so I remove the .ruff_cache directory first to reproduce the error

Shouldn't the removal of the wandb directory be enough to invalidate the module categorization cache? The fact that you need to manually remove the cache to get the correct results also seems wrong.

---

_Comment by @charliermarsh on 2024-04-04 00:44_

Diagnostics are cached on a per-file basis, where the "file" is the file in which the diagnostic is present. Changing _other_ files on the filesystem doesn't invalidate the cache.

Changing the _settings_ will also invalidate the cache. So if you add `wandb` as a `known-third-party` module (which is the suggested change), it will also reflect that change on re-run without clearing the cache mnanually.

---

_Comment by @bkad on 2024-04-04 01:17_

Say you were using a third party package in your requirements, but decided later to vendor it to make changes, turning it into a first party package. Then unless caches were manually cleaned you would get incorrect results from the isort check. Even worse, they could be inconsistent with CI checks, which can cause a lot of confusion. We ran into an issue like this today, and had to turn off caching as a result.

---

_Comment by @carschno on 2024-04-04 07:18_

> Say you were using a third party package in your requirements, but decided later to vendor it to make changes, turning it into a first party package. Then unless caches were manually cleaned you would get incorrect results from the isort check. Even worse, they could be inconsistent with CI checks, which can cause a lot of confusion. We ran into an issue like this today, and had to turn off caching as a result.

Honestly, I think this should be a separate issue about caching. It is only related in the sense that caching must somehow also handle the detection of third party folders. But if you propose improvements in the caching logic, I think it would make sense to do this separately from this issue (detection of source folders).

---

_Comment by @MichaReiser on 2024-04-04 07:50_

We're looking into changing how we cache data as part of our multifile analysis work. It will allow us to invalidate caches based on dependencies.

---

_Referenced in [astral-sh/ruff#12453](../../astral-sh/ruff/issues/12453.md) on 2024-07-22 14:40_

---

_Referenced in [astral-sh/ruff#14497](../../astral-sh/ruff/issues/14497.md) on 2024-11-20 20:08_

---

_Comment by @angerhang on 2025-01-28 13:19_

I would echo the same concern, it is really difficult for the end-users to pick up the error.

I had the same issue when using `wandb`. I ensured my local and remote CI tests have consistent environments but the differences still persist until I realised that a local folder name `wandb` is created. 

Better error message is needed if we can't have a quick fix easily.

---

_Referenced in [astral-sh/ruff#16376](../../astral-sh/ruff/issues/16376.md) on 2025-02-26 08:59_

---

_Comment by @MichaReiser on 2025-02-26 09:10_

We should prioritize resolving this issue. There is a known workaround, but it comes up regularly, and reproducing it is a ton of work for users (and us). 

---

_Label `priority:medium` added by @MichaReiser on 2025-02-26 12:23_

---

_Comment by @MichaReiser on 2025-04-23 19:47_

@dylwil3 do you have any ideas on how we could improve the `wandb` case described here? What I understand is that your most recent change won't help with this because it will still consider `wandb` as a possible namespace package (which is correct because Python would do the same)

---

_Referenced in [astral-sh/ruff-pre-commit#121](../../astral-sh/ruff-pre-commit/issues/121.md) on 2025-04-23 19:48_

---

_Comment by @carschno on 2025-04-24 11:17_

A slightly naive workaround would be to provide a default list of known third parties, including `wandb` for this specific issue. I see two downsides of such a list:
1. Adding maintenance effort
2. Not necessarily expected behaviour for new users

However, I don't see a generalizable solution, so perhaps such a specific list would be a starting point until it grows to an unmaintainable scale.


---

_Comment by @tboddyspargo on 2025-04-24 15:55_

> If a source folder is required to contain any `*.py` file, however, I cannot see how it would break current behaviour -- except for cases in which the sorting has now been incorrectly adapted to identified source folders. So perhaps requiring any `.py` file could be an intermediate fix, whereas the next major release requires an `__init__.py`?
> 
> If somebody could point me to the relevant source code, I could try and work on a PR.

Is this (requiring any `.py` file) a viable option? Perhaps I missed some discussion around this, but it sounded reasonable to me.

---

_Comment by @MichaReiser on 2025-04-24 15:59_

> Is this (requiring any .py file) a viable option? Perhaps I missed some discussion around this, but it sounded reasonable to me.

The problem with this is that Python doesn't require a `.py` file to consider a folder as a namespace package. But @dylwil3 has more details on the why

---

_Comment by @tboddyspargo on 2025-04-24 18:58_

> > Is this (requiring any .py file) a viable option? Perhaps I missed some discussion around this, but it sounded reasonable to me.
> 
> The problem with this is that Python doesn't require a `.py` file to consider a folder as a namespace package. But [@dylwil3](https://github.com/dylwil3) has more details on the why

Does it still require `.py` files somewhere in the hierarchy because if it's a candidate for a `namespace` package, presumably we'd have to look for files lower down to find out where the namespace package begins. Or are we constrained to looking at only immediate children `.py` files of the directory in question?

---

_Comment by @dylwil3 on 2025-04-24 19:18_

> Does it still require .py files somewhere in the hierarchy because if it's a candidate for a namespace package, presumably we'd have to look for files lower down to find out where the namespace package begins. Or are we constrained to looking at only immediate children .py files of the directory in question?

I think this is worth considering as sort of a compromise between how Python technically resolves the module and how the user's would expect (for the purposes of sorting). In theory it may have a performance impact if someone has a heavily nested non-Python folder that shares the name of a package.


Also I'm working on a longer response here to try to explain what's strange about the relationship between Python's module resolution and the notion of "first party" in import sorting - hopefully that can help clear some things up... or make everyone more confused, we'll see!

---

_Comment by @mickvangelderen on 2025-04-25 17:57_

This cost me half an hour today. I find it particularly surprising that ignored folders influence the categorization. 

```shell
❯ uv run ruff check --verbose . --select I --fix 2>&1 | grep flask_session
[2025-04-25][10:55:45][ignore::walk][DEBUG] ignoring <PROJECT_DIR>/flask_session: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("<PROJECT_DIR>/.gitignore"), original: "/flask_session/", actual: "flask_session", is_whitelist: false, is_only_dir: true })))
[2025-04-25][10:55:45][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'flask_session' as Known(FirstParty) (SourceMatch("<PROJECT_DIR>"))
```

---

_Referenced in [astral-sh/ruff#16565](../../astral-sh/ruff/pulls/16565.md) on 2025-04-26 20:24_

---

_Referenced in [astral-sh/ruff#18647](../../astral-sh/ruff/issues/18647.md) on 2025-06-12 14:11_

---

_Comment by @jonathanunderwood on 2025-08-04 06:41_

I just hit this today and it had me head scratching for a while. As an extra data point, isort doesn't exhibit this behaviour, and correctly detects first/third party related ordering, and so digging into isort's heuristics will likely help, though I haven't had time to do that yet.

---

_Comment by @dylwil3 on 2025-08-04 13:34_

@jonathanunderwood sorry to hear that! Could you help me reproduce this by describing the directory structure/imports where `ruff` and `isort` differ? As far as I know `isort` also suffers from the sort of dilemma in this thread - see, e.g.

- https://github.com/PyCQA/isort/issues/1619
- https://github.com/PyCQA/isort/issues/2143
- https://github.com/PyCQA/isort/issues/2101

---

_Comment by @MichaReiser on 2025-08-07 12:51_

A short-term workaround could be to add `wandb` to the `known_third_party` defaults, as this appears to be the cause of the confusing behavior for most users. Wdyt @dylwil3 ?

---

_Comment by @carschno on 2025-08-29 14:02_

> A short-term workaround could be to add `wandb` to the `known_third_party` defaults, as this appears to be the cause of the confusing behavior for most users. Wdyt [@dylwil3](https://github.com/dylwil3) ?

That was my suggestion [above](https://github.com/astral-sh/ruff/issues/10519#issuecomment-2827245863). While the downside (maintenance) remains, a more generic solution does not seem available.

---

_Comment by @jbcoe on 2025-08-29 14:25_

I wonder if skipping folders that are ignored by git (optionally) might be a nice generic fix for avoiding temporary files?

---

_Comment by @dylwil3 on 2025-09-09 13:07_

> A short-term workaround could be to add `wandb` to the `known_third_party` defaults, as this appears to be the cause of the confusing behavior for most users. Wdyt [@dylwil3](https://github.com/dylwil3) ?

Yeah I think we should do this - I'll make a PR!

---

_Referenced in [thousandbrainsproject/tbp.monty#557](../../thousandbrainsproject/tbp.monty/pulls/557.md) on 2025-11-14 10:23_

---

_Referenced in [astral-sh/ruff#21647](../../astral-sh/ruff/issues/21647.md) on 2025-11-28 21:25_

---

_Comment by @vadimkantorov on 2025-11-28 22:23_

IMO it can easily happen with some other python packages, and it will be equally confusing to debug. I suggest adding an option to somehow adjust how a package is treated local or not. And maybe somehow make isort variant which will not be dependent on local-ness of the module (and existence of any name-matching dirs) at all.

---

_Comment by @charliermarsh on 2025-11-28 22:27_

If you want to "not depend on local-ness", you can just remove the first-party and local sections -- this already exists:
```toml
[lint.isort]
section-order = [
    "future",
    "standard-library",
    "third-party",
]
```
Then all first-party and third-party imports will be grouped together.

---

_Comment by @MichaReiser on 2025-11-28 22:28_

You can use [no-sections](https://docs.astral.sh/ruff/settings/#lint_isort_no-sections) if you don't want isort to distinguish between first and third party packages

---

_Comment by @vadimkantorov on 2025-11-28 22:42_

This should help, thanks! Would be nice to maybe make a better default specifically for local dirs: e.g. when package exists both as third-party and as a local dir - do not propose import order fixes, and do not propose moving it out of the third-party section.

It should be relatively less common these days to intentionally actually have a local dir package to shadow a global package (given that these days it's so simple to have venvs, so it's easy to not put unneded global packages into the env in the first place). So maybe if a package exists as both third-party and local - actually consider it third-party.

And also, somehow python itself doesn't get confused and still loads the global wandb, and not breaks saying that the local folder `wandb` doesn't contain anything loadable. But maybe it requires more sophisticated resolution logic.

---

_Comment by @dylwil3 on 2025-11-29 01:26_

> when package exists both as third-party and as a local dir - do not propose import order fixes, and do not propose moving it out of the third-party section.

Ruff does not really have a way of knowing what third-party means beyond "not first party", essentially. That is: Ruff does not know what packages you have declared in your `pyproject.toml`, nor does it look for a virtual environment or anything like that.

One of the big ironies of import sorting is that it _feels_ like it should belong to the formatter, but when you look at the information it would need to understand in order to match user's expectations for first/third-party, it really needs to belong to something closer to a type checker or a package manager.

If only some organization built a formatter, package manager, _and_ a type checker 😄 

See #21520 for a possible way forward on this.

---

_Comment by @vadimkantorov on 2025-11-29 03:17_

Maybe making a heuristic, that a directory without `__init__.py` or without any python files considered not-first-party? How does Python itself decide that it should skip the local wandb dir?

I think the main problem here is coming up with a default setting which does not depend on existence of local folders and on CI / no-CI env... And it's really a painful thing to debug :(

---

_Comment by @dylwil3 on 2025-11-29 13:39_

> Maybe making a heuristic, that a directory without __init__.py or without any python files considered not-first-party? How does Python itself decide that it should skip the local wandb dir?

A directory without an `__init__.py` can still be a [namespace package](https://packaging.python.org/en/latest/guides/packaging-namespace-packages/). Checking recursively for Python files would be more likely to be a correct heuristic but could be more expensive to do.

What Python actually does will necessarily involve finding the virtual environment: As described in [the PEP](https://peps.python.org/pep-0420/#specification), Python will look first at your local directory structure and see the directory `foo` with no Python files in it, say. It will record that it found this as a "potential" namespace package. But then it will search your virtual environment. If it finds an honest Python package `foo` there, then that is the resolved module. If it doesn't, _then_ it will resolve to the local directory `foo` as an implicit namespace package.

So we couldn't mimic that _exact_ behavior unless we found your virtual environment. But having declared dependencies as in the `uv metadata` idea should give the same behavior (unless someone has an environment that doesn't match the declared one).

---
