```yaml
number: 16010
title: Backtick-quoted shortcut links in rule documentation
type: issue
state: closed
author: InSyncWithFoo
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-02-07T04:29:51Z
updated_at: 2025-02-10T08:54:23Z
url: https://github.com/astral-sh/ruff/issues/16010
synced_at: 2026-01-10T11:09:57Z
```

# Backtick-quoted shortcut links in rule documentation

---

_Issue opened by @InSyncWithFoo on 2025-02-07 04:29_

`FAST002`'s rule documentation reads:

`````markdown
The [FastAPI documentation] recommends the use of [`typing.Annotated`] for
<!-- ... -->
[fastAPI documentation]: https://fastapi.tiangolo.com/tutorial/query-params-str-validations/?h=annotated#advantages-of-annotated
[typing.Annotated]: https://docs.python.org/3/library/typing.html#typing.Annotated
`````

This is how PyCharm renders it:

![](https://github.com/user-attachments/assets/a249bd13-9a07-4436-ad59-dce038fa1611)

And GitHub:

> The [FastAPI documentation] recommends the use of [`typing.Annotated`] for
> <!-- ... -->
> [fastAPI documentation]: https://fastapi.tiangolo.com/tutorial/query-params-str-validations/?h=annotated#advantages-of-annotated
> [typing.Annotated]: https://docs.python.org/3/library/typing.html#typing.Annotated

CommonMark dingus [is no different](https://spec.commonmark.org/dingus/?text=The%20%5BFastAPI%20documentation%5D%20recommends%20the%20use%20of%20%5B%60typing.Annotated%60%5D%20for%0A%3C!--%20...%20--%3E%0A%5BfastAPI%20documentation%5D%3A%20https%3A%2F%2Ffastapi.tiangolo.com%2Ftutorial%2Fquery-params-str-validations%2F%3Fh%3Dannotated%23advantages-of-annotated%0A%5Btyping.Annotated%5D%3A%20https%3A%2F%2Fdocs.python.org%2F3%2Flibrary%2Ftyping.html%23typing.Annotated%0A):

```html
<p>The <a href="https://fastapi.tiangolo.com/tutorial/query-params-str-validations/?h=annotated#advantages-of-annotated">FastAPI documentation</a> recommends the use of [<code>typing.Annotated</code>] for</p>
```

---

[Shortcut reference links](https://spec.commonmark.org/0.31.2/#shortcut-reference-link) of the form ```[`something`]``` must have a corresponding [link reference definition](https://spec.commonmark.org/0.31.2/#link-reference-definition) whose [label](https://spec.commonmark.org/0.31.2/#link-label) is also backtick-quoted (related: #14348).

For reference, the matching algorithm is [defined](https://spec.commonmark.org/0.31.2/#matches) as:

> One label <b>matches</b> another just in case their normalized forms are equal. To normalize a label, strip off the opening and closing brackets, perform the <i>Unicode case fold</i>, strip leading and trailing spaces, tabs, and line endings, and collapse consecutive internal spaces, tabs, and line endings to a single space.

Here's what a fix would look like:

```diff
 The [FastAPI documentation] recommends the use of [`typing.Annotated`] for
 <!-- ... -->
 [fastAPI documentation]: https://fastapi.tiangolo.com/tutorial/query-params-str-validations/?h=annotated#advantages-of-annotated
-[typing.Annotated]: https://docs.python.org/3/library/typing.html#typing.Annotated
+[`typing.Annotated`]: https://docs.python.org/3/library/typing.html#typing.Annotated
```


---

_Renamed from "Backtick-quoted no-URI links in rule documentation" to "Backtick-quoted links with no explicit target in rule documentation" by @InSyncWithFoo on 2025-02-07 04:37_

---

_Label `documentation` added by @ntBre on 2025-02-07 04:50_

---

_Label `help wanted` added by @MichaReiser on 2025-02-07 07:42_

---

_Comment by @InSyncWithFoo on 2025-02-07 16:29_

```\[`[^`]+`\](?![\[(])``` returns 359 hits, while that of ```\[`(?!lint\.)[^`]+`\](?![\[(])``` is 67. This shows that the vast majority of broken links are option pseudo-links (and even then, there are still `line-length`, `target-version` and other top-level options that haven't been filtered out).

Fixable links can be found using ```\[`([^`]+)`\](?![\[(])(?=.*+(?>\R///.*)*\R/// \[\1)``` (or ```\[`([^`]+)`\](?![\[(])(?=.*\\n\[\1\]:)``` if you prefer working with `ruff rule --all --output-format=json`).

---

_Renamed from "Backtick-quoted links with no explicit target in rule documentation" to "Backtick-quoted shortcut links in rule documentation" by @InSyncWithFoo on 2025-02-08 01:19_

---

_Closed by @MichaReiser on 2025-02-10 08:54_

---

_Closed by @MichaReiser on 2025-02-10 08:54_

---
