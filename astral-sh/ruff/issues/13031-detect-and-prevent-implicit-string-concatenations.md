```yaml
number: 13031
title: Detect and prevent implicit string concatenations e.g. lists
type: issue
state: closed
author: KennethEnevoldsen
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-08-21T11:59:31Z
updated_at: 2024-08-22T08:57:47Z
url: https://github.com/astral-sh/ruff/issues/13031
synced_at: 2026-01-10T11:09:55Z
```

# Detect and prevent implicit string concatenations e.g. lists

---

_Issue opened by @KennethEnevoldsen on 2024-08-21 11:59_

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

A typical error I (and colleagues) run into is as follows:
```
mylist = ["a", "b" "c"] 
# results in:
mylist # ['a', 'bc']

# as opposed to the intended ["a", "b", "c"] 
```

While this pattern is often well known, this pattern can often lead to unintended side effects, which are hard to track and detect.


This suggestion is in line with [pep3126](https://peps.python.org/pep-3126/) (which was rejected due to "the feature to be removed isn’t all that harmful").

I have searched existing ruff rules and only found the ISC rules, which do not cover this pattern. The proposed pattern is in line with some of the ISC rules ([ISC001](https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/
)), while I would say it disagrees with ([ISC003](https://docs.astral.sh/ruff/rules/explicit-string-concatenation/))

---

_Renamed from "Detect and prevent implicit split concetatenations e.g. lists" to "Detect and prevent implicit string concetatenations e.g. lists" by @KennethEnevoldsen on 2024-08-21 12:01_

---

_Comment by @MichaReiser on 2024-08-21 12:08_

Can you help me understand how this rule would be different from ISC001 or why we should have both rules? We generally prefer **not** to have rules with overlapping concerns.

---

_Comment by @KennethEnevoldsen on 2024-08-21 12:15_

In the example above the intention is:

```
mylist = ["a", "b" "c"] 
```
ISC001 would suggest (according to the documentation and notably in autofixes). That it should be:

```
mylist = ["a", "bc"] # correctly reflects how it is evaluated, but not the most likely intention
``` 

While I would prefer:
```
mylist = ["a", "b", "c"] # correctly reflects the intention
```


---

_Comment by @KennethEnevoldsen on 2024-08-21 12:21_

In general, I would prefer not to have any implicit string concatenation in my code, which at least in my reading of the ISC it argues for due to implicit string concatenation being more readable. This might be true, but at least it can often carry bugs so I prefer to avoid it if possible. 

---

_Comment by @MichaReiser on 2024-08-21 12:27_

> In general, I would prefer not to have any implicit string concatenation in my code, which at least in my reading of the ISC it argues for due to implicit string concatenation being more readable.

Using ISC001 and ISC002 should give you the desired behavior that all implicitly concatenated strings are flagged. 

> ISC001 would suggest (according to the documentation and notably in autofixes). That it should be:

Thank's. That's helpful. This is related to #13014 Overall it seems that there could be an addition of one more implicit string concatenation rule that detects suspicious usages (vs denying it for stylistic reasons). Although it isn't clear to me how to solve the overlap with ISC001, and ISC002

---

_Label `rule` added by @MichaReiser on 2024-08-21 12:27_

---

_Label `needs-decision` added by @MichaReiser on 2024-08-21 12:27_

---

_Comment by @KennethEnevoldsen on 2024-08-21 14:28_

> Using ISC001 and ISC002 should give you the desired behavior that all implicitly concatenated strings are flagged.

The auto fix does still seem problematic to me. I can of course just disable it.

> vs denying it for stylistic reasons

I wouldn't call denying a pattern that often carries bugs a stylistic reason - the discussion around the pep acknowledged this as well, with the reason for not removing it mainly concerning backward compatibility. A choice that I fully understand, but where it is hard to remove it is easy to prevent using linters.

> Although it isn't clear to me how to solve the overlap with ISC001, and ISC002

I do think the change is inherently incompatible. ISC002 and ISC003 prefer implicit concatenation over explicit concatenation (ISC001 is fully compatible at least during linting). Where I would argue anytime you see a string concatenation in a list, set, etc. (`["a", "b" "c"]`) it is much more likely to be a bug than an intended string concatenation.


---

_Comment by @AlexWaygood on 2024-08-21 14:42_

The exact autofix behaviour you want from ISC001 here likely depends on whether the single-line implicit concatenation was introduced by a human or by an autoformatter. If the implicitly concatenated string was added by a human, then it's almost certainly a mistake, and the human likely meant to put a comma in between the two strings (as you say @KennethEnevoldsen). But if it was inserted by an autoformatter, it was probably previously a multiline implicitly concatenated string that the autoformatter realised was needleslly spread out over multiple lines; in that case, the proper autofix is to merge the two strings, as `ISC001` currently does.

Ideally we'd just fix Black and the Ruff formatter so that they never create single-line implicitly concatenated strings in the first place. But the `ISC001` rule (originally as a flake8 plugin) was originally created to address an issue where Black would often undesirably collapse multiline implicitly concatenated strings into single-line implicitly concatenated strings. So that's probably the motivation for the current autofix behaviour.

Unfortunately it's not really possible for a linter rule to know whether the implicit concatenation was introduced by a human or an autoformatter...!

---

_Comment by @KennethEnevoldsen on 2024-08-21 15:30_

Thanks for the clarification @AlexWaygood, this gives a lot of context to the current formulation of `ISC001`. From the warning it also seems to conflict with the ruff formatter:

```
warning: The following rules may cause conflicts when used with the formatter: `ISC001`.
```

Just to be clear, I don't believe the auto fix behavior `["a", "b" "c"] -> ["a", "b", "c"]` should be implemented.

I (and I imagine others as well) would prefer a set of rules with simply raise a linting error on implicit string concatenation (not only in a single line). This resolves the conflict with the formatter (as far as I understand) and makes parsing simpler. Naturally, it would be optional and incompatible with ISC003.

---

_Comment by @AlexWaygood on 2024-08-21 15:40_

So, I _think_ you can already get what you're asking for @KennethEnevoldsen using this configuration:

```toml
[tool.ruff.lint]
extend-select = ["ISC"]
unfixable = ["ISC001"]

[tool.ruff.lint.flake8-implicit-str-concat]
allow-multiline = false
```

That will result in Ruff complaining about both single-line and multiline implicitly concatenated strings, and it will switch off the autofix for ISC001 (even if you run Ruff with `--fix`).

---

_Comment by @AlexWaygood on 2024-08-21 15:43_

(Docs on that setting are here: https://docs.astral.sh/ruff/settings/#lint_flake8-implicit-str-concat_allow-multiline)

---

_Renamed from "Detect and prevent implicit string concetatenations e.g. lists" to "Detect and prevent implicit string concatenations e.g. lists" by @AlexWaygood on 2024-08-22 07:14_

---

_Comment by @KennethEnevoldsen on 2024-08-22 08:35_

Thanks, @AlexWaygood that does indeed resolve the issue (couldn't find a case where it failed). I should have noticed that sooner. I went through the documentation to check if this might be a chance to improve the documentation, but there is nothing that I would change as is.

I will close this issue for now. Thanks, @AlexWaygood and @MichaReiser for the help.

---

_Closed by @KennethEnevoldsen on 2024-08-22 08:35_

---

_Comment by @hugovk on 2024-08-22 08:47_

> Ideally we'd just fix Black and the Ruff formatter so that they never create single-line implicitly concatenated strings in the first place. But the `ISC001` rule (originally as a flake8 plugin) was originally created to address an issue where Black would often undesirably collapse multiline implicitly concatenated strings into single-line implicitly concatenated strings. 

Black has a fix in preview mode to prevent some of the ISC-related issues, but it's not been ready for the past couple of annual style releases. See https://github.com/psf/black/issues/2188. 

---

_Comment by @MichaReiser on 2024-08-22 08:57_

@hugovk we plan on implementing a simpler solution that we feel more confident in stabilizing (the mentioned black preview style has been in preview for a long term). See https://github.com/astral-sh/ruff/issues/9457

---
