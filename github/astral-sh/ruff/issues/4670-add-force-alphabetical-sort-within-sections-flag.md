---
number: 4670
title: add force-alphabetical-sort-within-sections flag to isort settings
type: issue
state: open
author: alexdauenhauer
labels:
  - isort
  - configuration
assignees: []
created_at: 2023-05-26T14:49:00Z
updated_at: 2024-04-28T17:08:04Z
url: https://github.com/astral-sh/ruff/issues/4670
synced_at: 2026-01-07T13:12:14-06:00
---

# add force-alphabetical-sort-within-sections flag to isort settings

---

_Issue opened by @alexdauenhauer on 2023-05-26 14:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

~While I personally like `isort` better, my company uses [reorder-python-imports](https://pypi.org/project/reorder-python-imports/) so I am wondering if there is already support for this and I missed it in the docs, or if support could be added? I am loving this package and would love to have it auto-run the import sorting in the format I need.~

Actually I think it is better to just add `force-alphabetical-sort-within-sections` flag to isort settings


---

_Comment by @alexdauenhauer on 2023-05-26 15:57_

actually it turns out I can almost achieve the same functionality, but the isort arg that I need is `force-alphabetical-sort-within-sections` and ruff does not yet support this

---

_Comment by @JonathanPlasse on 2023-05-26 16:20_

Can you share the configuration?
A profile `reporder-python-imports` could be added.

---

_Comment by @alexdauenhauer on 2023-05-26 17:33_

well now I think what would actually be better than adding support for reorder-python-imports, would be to just add the `force-alphabetical-sort-within-sections` flag for available `isort` args. I think isort can do everything that reorder-python-imports can do, but not all the flags for isort are available to enable that functionality

---

_Renamed from "Support for reorder-python-imports" to "add force-alphabetical-sort-within-sections flag to isort settings" by @alexdauenhauer on 2023-05-26 17:34_

---

_Label `isort` added by @charliermarsh on 2023-05-26 18:42_

---

_Label `configuration` added by @charliermarsh on 2023-05-26 18:42_

---

_Comment by @sanmai-NL on 2023-06-22 14:26_

@alexdauenhauer Does this solve it: https://beta.ruff.rs/docs/settings/#isort-force-sort-within-sections?

---

_Comment by @alexdauenhauer on 2023-06-22 19:08_

@sanmai-NL unfortunately not. The sorting rules are slightly different. sort-within-sections prioritizes case over alphanumeric (e.g. `MY_CONSTANT` will be sorted on top of `alphabetical_method`) 

---

_Comment by @perezzini on 2023-08-09 15:19_

Any updates about this? Thanks!

---

_Comment by @owenlamont on 2023-10-29 22:23_

I'd like this too, I'm trying to get Ruff adopted where I work but this is one area where a lot of code gets formatted differently to our current isort configuration.

---

_Comment by @owenlamont on 2023-11-01 11:25_

Update: Actually I don't think this is an issue anymore. Once I noticed the _case-sensitive_ argument which I set to _false_ and I fixed and tweaked some other ruff configuration I was able to exactly reproduce my old isort configuration sorting behaviour that had force_alphabetical_sort_within_sections set to true.

---

_Comment by @Pierre-Sassoulas on 2024-01-31 13:26_

@owenlamont do you mind sharing your configuration to mimic `force_alphabetical_sort_within_sections`, please ? I didn't manage to recreate it from the description.

---

_Referenced in [pytest-dev/pytest#11901](../../pytest-dev/pytest/pulls/11901.md) on 2024-01-31 13:37_

---

_Comment by @owenlamont on 2024-01-31 21:44_

Hi @Pierre-Sassoulas - this was my Ruff isort config taken from my pyproject.toml (note I excluded known-first-party and known-third-party modules which will be specific to your repo). This sorting was close to but not quite identical to isort. I think there were some minor differences with Ruff sorting on full relative import paths (which I personally liked) - I think the difference was something like that - I don't exactly remember but most of the time the import sort order came out identical to how we had isort configured.

```toml
[tool.ruff.isort]
case-sensitive = false
combine-as-imports = true
force-sort-within-sections = true
known-first-party = []
known-third-party = []
lines-after-imports = 2
order-by-type = false
section-order = [
    "future",
    "standard-library",
    "third-party",
    "first-party",
    "local-folder"
]
```

This closely matched the behaviour of the old isort config:

```toml
[tool.isort]
profile = "black"
multi_line_output = 3
force_sort_within_sections = true
force_alphabetical_sort_within_sections = true
sections = ["FUTURE", "STDLIB", "THIRDPARTY", "FIRSTPARTY", "LOCALFOLDER"]
known_first_party = []
known_thirdparty = []
skip_gitignore=true
lines_after_imports=2
combine_as_imports=true
```

---

_Referenced in [astral-sh/ruff#6190](../../astral-sh/ruff/issues/6190.md) on 2024-02-01 05:47_

---

_Comment by @Pierre-Sassoulas on 2024-02-01 10:04_

It's working, thank you a lot @owenlamont !

> The sorting rules are slightly different. sort-within-sections prioritizes case over alphanumeric (e.g. MY_CONSTANT will be sorted on top of alphabetical_method)

The specific option that fix this is ``order-by-type = false``.

I guess the issue can be closed then.






---

_Comment by @Jari27 on 2024-03-06 13:42_

I have a similar problem that I cannot seem to solve with the current settings on v0.2.1 (although I cannot find anything in the changelogs that indicate this was solved in v0.2.2 or v0.3.0).

I have the following 5 imports:
```python
from pyspark.sql import Column, DataFrame
from pyspark.sql import functions as F
from pyspark.sql import SparkSession
from pyspark.sql import types as T
```

These are sorted by isort with the following config:
```ini
[tool.isort]
line_length = 120
multi_line_output = 3
force_alphabetical_sort_within_sections = "True"
force_sort_within_sections = "False"
profile = "black"
```

I cannot seem to get the same behaviour with ruff, as it always combines the first and third line into this:
```python
from pyspark.sql import Column, DataFrame, SparkSession
from pyspark.sql import functions as F
from pyspark.sql import types as T
```

with the following config:
```ini
[tool.ruff.lint.isort]
force-sort-within-sections = false
order-by-type = false
case-sensitive = false
```

Am I misconfiguring anything? It seems like the alphabetical sort is sorting AFTER grouping, instead of before. And I cannot seem to turn this off.

---

_Comment by @samdoran on 2024-04-22 20:33_

@Jari27 I think you want `force-single-line = true`.

```
from pyspark.sql import Column
from pyspark.sql import DataFrame
from pyspark.sql import functions as F
from pyspark.sql import SparkSession
from pyspark.sql import types as T
```

---

_Referenced in [astral-sh/ruff#10928](../../astral-sh/ruff/issues/10928.md) on 2024-04-22 20:35_

---

_Comment by @Jari27 on 2024-04-26 11:38_

@samdoran Thanks, that is a lot closer but still not fully compatible. isort (`isort==5.10.1`) instead puts `Column` and `DataFrame` on the same line. 

---

_Comment by @charliermarsh on 2024-04-26 13:50_

Yes we don't support that.

---

_Comment by @charliermarsh on 2024-04-26 13:50_

It's intentional and documented here: https://docs.astral.sh/ruff/faq/#how-does-ruffs-import-sorting-compare-to-isort

---

_Comment by @Jari27 on 2024-04-28 17:08_

Ah that's fair. Not sure how I missed it! Thanks for the help!

---

_Referenced in [apache/superset#28267](../../apache/superset/pulls/28267.md) on 2024-04-29 17:45_

---
