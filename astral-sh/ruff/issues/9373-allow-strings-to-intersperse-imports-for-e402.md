---
number: 9373
title: Allow strings to intersperse imports for E402
type: issue
state: closed
author: XChikuX
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-01-03T00:43:29Z
updated_at: 2024-10-07T07:38:22Z
url: https://github.com/astral-sh/ruff/issues/9373
synced_at: 2026-01-10T01:22:49Z
---

# Allow strings to intersperse imports for E402

---

_Issue opened by @XChikuX on 2024-01-03 00:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

My expectations of Ruff (E402) don't match the error it throws

Example: (**_this should be valid_**)

```py
from io import BytesIO
from typing import (
    AsyncIterable,
    Awaitable,
    Callable,
    Tuple,
    List,
    Dict,
    Union,
    Any,
)

"""                     STRAWBERRY IMPORTS                            """
from fastapi import BackgroundTasks
import strawberry
from strawberry.fastapi import BaseContext
from strawberry.types import Info as _Info
```


However I see vscode through E402 errors, for the imports after the comments.

The [Docs](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/) don't satisfactorily cover this edge case either.

---

_Comment by @charliermarsh on 2024-01-03 01:47_

I believe this is intentional. What's the motivation for using a string rather than a comment in that position?

---

_Label `question` added by @charliermarsh on 2024-01-03 01:48_

---

_Comment by @XChikuX on 2024-01-03 05:04_

It helps me differentiate between code sections. I find it very useful when a file starts to get big.

Splitting it into more files causes unnecessary overhead from imports. And I have regular comments in other areas.

---

_Comment by @trag1c on 2024-01-03 09:03_

Wouldn't a more visible comment like
```py
#########################################################################
###                     STRAWBERRY IMPORTS                            ###
#########################################################################
```
suffice?

---

_Comment by @XChikuX on 2024-01-03 17:39_

Unfortunately no, different comment types have different coloration.

In my vscode "#" type comments are green. And 3 quoted comments like the above mentioned are orange/red.

I personally have a much easier time shifting through my code. When I can sort by color in my brain.

Your argument could follow up by saying, *we don't cater to individual preferences*

However, if I prefer this, I'm sure there are many developers out there who think like I do and just haven't expressed their opinions on github.

Since this change doesn't affect other coders adversely in anyway. I don't see any reason not to implement it unless there is a good reason not to.

I start my day coding with an annoying warning everyday. I'd very much like it to go away. 

---

_Comment by @johnslavik on 2024-01-03 17:51_

> I start my day coding with an annoying warning everyday. I'd very much like it to go away.

You can always suppress the warning with `# noqa: E402`.
The practice you've mentioned isn't very common. Triple quoted strings, besides being strings, are primarily meant to serve as a documentation. If the problem is with colors, I would recommend looking for a different theme/tweaking your current one to make it easier for you to work with. I think I had read quite a lot of Python code already and I've never seen someone using strings that way.


---

_Comment by @charliermarsh on 2024-01-03 18:06_

I'm open to changing if there's additional support for it, so I'll probably leave this open for now. For me it's not quite correct (e.g., arguably misleading since it's not captured as a docstring by Python itself) and not adherent to PEP 8, but not in a way that's hugely critical.

---

_Comment by @tjkuson on 2024-01-03 18:27_

If the aim is code separation, I am unclear why that suggests using a string: strings are typically more common and larger than comments. Further, strings are usually just ordinary parts of code implementation, whereas comments are more likely to draw the eye of the reader as something to note. Thus, wouldn't using a comment would be *better* suited to delineate code sections?

---

_Comment by @XChikuX on 2024-01-04 21:45_

> > I start my day coding with an annoying warning everyday. I'd very much like it to go away.
> 
> You can always suppress the warning with `# noqa: E402`. The practice you've mentioned isn't very common. Triple quoted strings, besides being strings, are primarily meant to serve as a documentation. If the problem is with colors, I would recommend looking for a different theme/tweaking your current one to make it easier for you to work with. I think I had read quite a lot of Python code already and I've never seen someone using strings that way.

I've added a:
```config
[tool.ruff.lint.per-file-ignores]
"app/common.py" = ["E402"]
```

There was only one file where my imports were a bit harder to manage and needed the separation I'd asked for.

Thank you for the suggestion @bswck 

This fits my needs so far. However, I still don't see why E402 needs to cover docstring issues. Perhaps it wasn't considered as a part of the original PEP 8?

@charliermarsh I appreciate you being open to different perspectives. If this has no support for a few months feel free to close it.

PS, I found [a missing flake8 feature](https://stackoverflow.com/questions/64428794/flake8-disable-linter-only-for-a-block-of-code). This could be a missing feature in ruff too (NOTE: *I only did a preliminary search of the docs, I could have missed it*)

@tjkuson I use comments for very serious portions of code and giving the dev important information and considerations. I try to squeeze in obvious information in `log.info`'s

Code separation isn't something I consider as serious as logic flows. Do let me know if this is just a subjective opinion.




---

_Comment by @tjkuson on 2024-01-04 23:43_

> Code separation isn't something I consider as serious as logic flows. Do let me know if this is just a subjective opinion.

IMO, yeah, this is subjective. I've mostly seen/used code separation to indicate something exceptional to the reader and catch their eye (e.g., because the code is complex or long, so sharp visual markers make it easier to parse).

EDIT: My comment was also less about hierarchy of importance and more distinction; to me, strings look too much like runtime code for it to function well as something that separates code. But my main point here is that I think your style preference is subjective and non-idiomatic.

---

_Comment by @XChikuX on 2024-01-05 00:29_

@tjkuson This just looks really clean to me. If you prefer something else, power to you 

![image](https://github.com/astral-sh/ruff/assets/5894493/0f870cab-1551-4dc7-a5ee-a61ffb0e765e)



---

_Comment by @johnslavik on 2024-01-05 00:52_

> @tjkuson This just looks really clean to me. If you prefer something else, power to you
> 
> ![image](https://private-user-images.githubusercontent.com/5894493/294331419-0f870cab-1551-4dc7-a5ee-a61ffb0e765e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDQ0MTU3NTAsIm5iZiI6MTcwNDQxNTQ1MCwicGF0aCI6Ii81ODk0NDkzLzI5NDMzMTQxOS0wZjg3MGNhYi0xNTUxLTRkYzctYTVlZS1hNjFmZmIwZTc2NWUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDEwNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDAxMDVUMDA0NDEwWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9NGJkODNjYTUwNjFiOWEyOTI2M2U0NzczN2ZiNzJiYWIzZTdjYmRlZDdjNjQxZGZiMjEwMDQwNzEwOGVmNGVmNyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ.Bf4O51S1PWrl4f_skou0rmZGf-p5lcC9Wlg1AKQoGYs)

This doesn't follow [PEP 8](https://peps.python.org/pep-0008/#imports).
A standard is a standard.

Of course, your preference is your preference. But for the sake of readability and overall convenience when working with your code, I would recommend sticking to the PEP8 __standard__. Ruff is pretty good at helping in doing that.

> PS, I found [a missing flake8 feature](https://stackoverflow.com/questions/64428794/flake8-disable-linter-only-for-a-block-of-code). This could be a missing feature in ruff too (NOTE: I only did a preliminary search of the docs, I could have missed it)

This is off-topic. Maybe open a separate issue instead? ;)

---

_Comment by @XChikuX on 2024-01-05 02:00_

> A standard is a standard.

I question all standards. Always have, always will.

---

_Comment by @johnslavik on 2024-01-05 02:08_

> > A standard is a standard.
> 
> I question all standards. Always have, always will.

Unlike Ruff :P

---

_Renamed from "Ruff (E402) bug" to "Allow strings to intersperse imports for E402" by @charliermarsh on 2024-01-05 04:38_

---

_Label `question` removed by @charliermarsh on 2024-01-05 04:38_

---

_Label `rule` added by @charliermarsh on 2024-01-05 04:38_

---

_Label `needs-decision` added by @charliermarsh on 2024-01-05 04:38_

---

_Comment by @XChikuX on 2024-10-03 09:23_

> > > I start my day coding with an annoying warning everyday. I'd very much like it to go away.
> > 
> > You can always suppress the warning with `# noqa: E402`. The practice you've mentioned isn't very common. Triple quoted strings, besides being strings, are primarily meant to serve as a documentation. If the problem is with colors, I would recommend looking for a different theme/tweaking your current one to make it easier for you to work with. I think I had read quite a lot of Python code already and I've never seen someone using strings that way.
> 
> I've added a:
> ```config
> [tool.ruff.lint.per-file-ignores]
> "app/common.py" = ["E402"]
> ```
> 
> There was only one file where my imports were a bit harder to manage and needed the separation I'd asked for.
> 
> Thank you for the suggestion @bswck 
> 
> This fits my needs so far. However, I still don't see why E402 needs to cover docstring issues. Perhaps it wasn't considered as a part of the original PEP 8?
> 
> @charliermarsh I appreciate you being open to different perspectives. If this has no support for a few months feel free to close it.
> 
> PS, I found [a missing flake8 feature](https://stackoverflow.com/questions/64428794/flake8-disable-linter-only-for-a-block-of-code). This could be a missing feature in ruff too (NOTE: *I only did a preliminary search of the docs, I could have missed it*)
> 
> @tjkuson I use comments for very serious portions of code and giving the dev important information and considerations. I try to squeeze in obvious information in `log.info`'s
> 
> Code separation isn't something I consider as serious as logic flows. Do let me know if this is just a subjective opinion.
> 
> 
> 

The ignore rule is working quite well for me.

@charliermarsh feel free to close this. 


---

_Closed by @MichaReiser on 2024-10-07 07:38_

---
