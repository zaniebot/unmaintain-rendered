```yaml
number: 14496
title: "[`pydoclint`] Extend `docstring-missing-exception` to empty exception descriptions (`DOC501`)"
type: pull_request
state: closed
author: sbrugman
labels:
  - rule
assignees: []
draft: true
base: main
head: doc501-empty-exception
created_at: 2024-11-20T19:02:30Z
updated_at: 2025-12-10T17:45:25Z
url: https://github.com/astral-sh/ruff/pull/14496
synced_at: 2026-01-10T16:42:11Z
```

# [`pydoclint`] Extend `docstring-missing-exception` to empty exception descriptions (`DOC501`)

---

_Pull request opened by @sbrugman on 2024-11-20 19:02_



<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #14494.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

`cargo test`. Awaiting ecosystem checks.


---

_Comment by @github-actions[bot] on 2024-11-20 19:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/util.py#L41'>src/bokeh/client/util.py:41:5:</a> DOC501 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/util.py#L68'>src/bokeh/client/util.py:68:5:</a> DOC501 Raised exception `ValueError` missing from docstring
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC501 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-11-20 20:24_

Let's wait with merging this until after the 0.8 release

---

_Label `rule` added by @MichaReiser on 2024-11-20 20:24_

---

_Comment by @sbrugman on 2024-11-21 07:22_

Bokeh is a false positive. They use google-style, but start exception descriptions on a newline.

---

_Comment by @sbrugman on 2024-11-21 13:15_

Reference that newlines are valid after the colon in Google-style docstrings: https://google.github.io/styleguide/pyguide.html#doc-function-args

---

_Comment by @MichaReiser on 2024-11-25 10:18_

> Reference that newlines are valid after the colon in Google-style docstrings: [google.github.io/styleguide/pyguide.html#doc-function-args](https://google.github.io/styleguide/pyguide.html#doc-function-args)

I'm now slightly confused. Doesn't that suggest that the Bokeh ecosystem results *aren't* a false positives?



---

_Comment by @MichaReiser on 2024-11-25 10:22_

I noticed that we have `D414` which explicitly flags empty sections. 

```py
def calculate_speed(distance: float, time: float) -> float:
    """Calculate speed as distance divided by time.

    Args:

    Returns:

    Raises:
        ValueError:

    """
    raise ValueError
    return distance / time
```

Both the `Args` and `Returns` section are flagged by `D414` but the `ValueError` isn't. Should we extend `D414` to detect missing exception documentation instead?

I suspect that the counter-argument is that the rule also doesn't raise for arguments with missing documentation:

```py
def calculate_speed(distance: float, time: float) -> float:
    """Calculate speed as distance divided by time.

    Args:
        distance:
        time:

    Returns:

    Raises:
        ValueError:

    """
    raise ValueError
    return distance / time
```

---

_Comment by @sbrugman on 2024-11-25 10:58_

> I'm now slightly confused. Doesn't that suggest that the Bokeh ecosystem results _aren't_ a false positives?

Bokeh has documentation, just on a new line which is valid according to the Google-style. No violation should be raised. There are two new ones. 

---

_Converted to draft by @sbrugman on 2024-11-25 22:57_

---

_Comment by @MichaReiser on 2025-12-10 17:45_

Thanks for your contribution. I'll close this PR because it has become stale. Please don't hesitate to resubmit the PR and reference this PR if you plan to keep working on this feature.

---

_Closed by @MichaReiser on 2025-12-10 17:45_

---
