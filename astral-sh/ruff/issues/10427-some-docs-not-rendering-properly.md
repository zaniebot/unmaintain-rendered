```yaml
number: 10427
title: Some docs not rendering properly
type: issue
state: closed
author: augustelalande
labels:
  - bug
  - documentation
assignees: []
created_at: 2024-03-15T22:03:35Z
updated_at: 2024-03-21T16:34:00Z
url: https://github.com/astral-sh/ruff/issues/10427
synced_at: 2026-01-10T11:09:52Z
```

# Some docs not rendering properly

---

_Issue opened by @augustelalande on 2024-03-15 22:03_

https://docs.astral.sh/ruff/rules/too-many-blank-lines/ (among others) is not rendering properly, even though the markdown seems fine.

The list is not rendered as a bullet list.
![image](https://github.com/astral-sh/ruff/assets/5696119/e853f9f8-a1dc-4335-909d-968a9385bfb9)

The links to settings do not link.
![image](https://github.com/astral-sh/ruff/assets/5696119/ac798c72-bc5f-45c6-b5ea-760ea7d525f9)



---

_Renamed from "Some docs not rendering properlu" to "Some docs not rendering properly" by @augustelalande on 2024-03-15 22:03_

---

_Label `bug` added by @zanieb on 2024-03-15 22:37_

---

_Label `documentation` added by @zanieb on 2024-03-15 22:37_

---

_Comment by @zanieb on 2024-03-15 22:38_

Thanks for flagging this!

---

_Comment by @augustelalande on 2024-03-17 04:59_

Ok so there are two issues at play here.
1. Sentences followed by lists
```
list:
- item1
- item2
```
will not be rendered as lists by python-mardown (https://github.com/Python-Markdown/markdown/issues/1442), which is what mkdocs uses to transcompile the mardown to html. Instead a newline needs to be added between the sentence and items.
```
list:

- item1
- item2
```
Annoyingly, this is different from the behavior of many other markdown processors (including the one used on github) so most people will not be expecting this.

2. The current documentation preprocessor is very strict about linking settings, only lines under the `options` header, using a `-` list indicator will be processed and linked correctly, so what was attempted in the misbehaving doc will not work.
https://github.com/astral-sh/ruff/blob/608df9a1bc0e6025049add877d1d833f1739e966/crates/ruff_dev/src/generate_docs.rs#L116-L133

---

_Comment by @augustelalande on 2024-03-17 05:01_

Regarding issue 1, it is a fairly easy fix to change the comments in the code docs, but might be difficult to ensure all of them are fixed.

Secondly, because the behavior is so different to what people are used to I feel something should be done to prevent this in the future.

---

_Comment by @augustelalande on 2024-03-17 05:03_

Regarding issue 2, the doc prepossor could be expanded to accept more patterns, but it depends how frequent this type of links will appear outside the `options` headings.

---

_Comment by @zanieb on 2024-03-17 14:25_

Yeah this is a little annoying, maybe we can run our markdown formatter on the generated documentation? I wonder if it adds spacing.

In the meantime, it seems like adding the missing spaces makes sense.

I'm not sure about the automatic linking, @charliermarsh do you have context on the original intent of limiting the scope?

---

_Comment by @charliermarsh on 2024-03-17 15:17_

I think it’s just because the original intent was for that to support the Options section which always has that format. We can expand the scope a bit though.

---

_Comment by @augustelalande on 2024-03-17 20:37_

> Yeah this is a little annoying, maybe we can run our markdown formatter on the generated documentation? I wonder if it adds spacing.
> 
> In the meantime, it seems like adding the missing spaces makes sense.
> 
> I'm not sure about the automatic linking, @charliermarsh do you have context on the original intent of limiting the scope?

Are you already running a markdown formatter in the project?

---

_Comment by @augustelalande on 2024-03-17 20:45_

If not we could try running https://github.com/executablebooks/mdformat which seems to do the trick.

---

_Comment by @augustelalande on 2024-03-17 20:51_

For the option processing we could match this regex ```\[`(.*?)\`\]\[\1\]``` to generate the option reference?

---

_Comment by @zanieb on 2024-03-17 23:36_

We use a couple 

https://github.com/astral-sh/ruff/blob/main/.pre-commit-config.yaml#L20-L41

It might require a tweak to run them on the generated documentation

---

_Comment by @augustelalande on 2024-03-18 04:55_

@zanieb I started implementing a check using mdformat, but as it is right now pretty much every doc has multiple violations (mostly missing or extra newlines). For example:
```
Rule `zip-without-explicit-strict` docs are not formatted. Either format the rule or add to `KNOWN_FORMATTING_VIOLATIONS`. The section should be rewritten to:
---

+++

@@ -1,12 +1,15 @@

 # zip-without-explicit-strict (B905)
+
 Derived from the **flake8-bugbear** linter.

 Fix is always available.

 ## What it does
+
 Checks for `zip` calls without an explicit `strict` parameter.

 ## Why is this bad?
+
 By default, if the iterables passed to `zip` are of different lengths, the
 resulting iterator will be silently truncated to the length of the shortest
 iterable. This can lead to subtle bugs.
@@ -15,19 +18,23 @@

 non-uniform length.

 ## Example
+
 \```python
 zip(a, b)
 \```

 Use instead:
+
 \```python
 zip(a, b, strict=True)
 \```

 ## Fix safety
+
 This rule's fix is marked as unsafe for `zip` calls that contain
 `**kwargs`, as adding a `check` keyword argument to such a call may lead
 to a duplicate keyword argument error.

 ## References
+
 - [Python documentation: `zip`](https://docs.python.org/3/library/functions.html#zip)
```

So if we go the route of enforcing markdown style on rule docs it will require a lot of edits. Alternatively, if we only care about enforcing a line break around lists, this could be done with a simple python check. Something like:
`if line followed by '-' or '*' then raise error`

---

_Comment by @augustelalande on 2024-03-19 00:17_

Another option would be to format the docs as part of the generation process

---

_Comment by @augustelalande on 2024-03-19 13:33_

@zanieb what do you think between these three options? Remember the goal is just to ensure that lists get rendered properly.
1. Enforce a strict markdown format with `mdformat` across the board. Requiring contributors to modify their docs before merging.
2. Enforce a reduced markdown format with some custom python (namely line breaks around lists). Still requiring contributors to modify their docs before contributing.
3. Automatically format docs with `mdformat` when rendering the website.

---

_Comment by @zanieb on 2024-03-19 14:29_

I honestly was imagining (3) but we'd probably want to run that check on pull requests too in case someone breaks the formatter in which case it would require manual intervention.

---

_Comment by @augustelalande on 2024-03-19 14:36_

What do you mean "breaks the formatter", like raise an unexpected error?

---

_Comment by @zanieb on 2024-03-19 14:37_

Yeah, idk if something goes wrong like it formats and can't render or the formatter errors.

---

_Comment by @augustelalande on 2024-03-19 14:39_

So adding it as part of generate_mkdocs.py would make sense, since that gets run for every PR.

---

_Comment by @charliermarsh on 2024-03-21 00:31_

So some of this is resolved, but I'm still seeing invalid references like this (possibly new ones?):

![Screenshot 2024-03-20 at 8 30 42 PM](https://github.com/astral-sh/ruff/assets/1309177/bbf46d17-c238-4884-a535-e846223df8bc)


---

_Comment by @augustelalande on 2024-03-21 00:34_

Ya I was gonna work on that next.

The problem is that when someone attempts a reference to a setting, but doesn't make an options section with that setting, the reference doesn't get linked. My approach is gonna be to check any reference which matches the pattern [`word`][word], check if it's a valid setting, and if it is link it.

---

_Comment by @charliermarsh on 2024-03-21 00:41_

Thanks, I fixed those specific docs here: https://github.com/astral-sh/ruff/pull/10498

---

_Comment by @augustelalande on 2024-03-21 00:43_

What are your thoughts on my comment though. Should we allow linking settings without an options section? If not should we flag attempts to do so?

---

_Comment by @charliermarsh on 2024-03-21 00:47_

Honestly, I'm fine with either. We should _at least_ flag references that are missing from the `Options` section.

---

_Comment by @augustelalande on 2024-03-21 00:48_

Ok I will work on that

---

_Closed by @charliermarsh on 2024-03-21 16:34_

---
