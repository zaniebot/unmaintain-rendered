---
number: 9389
title: Add support for optional trailing comma
type: issue
state: closed
author: 17Reset
labels:
  - formatter
  - style
assignees: []
created_at: 2024-01-04T05:55:51Z
updated_at: 2025-04-23T14:32:48Z
url: https://github.com/astral-sh/ruff/issues/9389
synced_at: 2026-01-07T13:12:15-06:00
---

# Add support for optional trailing comma

---

_Issue opened by @17Reset on 2024-01-04 05:55_

When I format my code using ruff format, after the format, there is always a comma after the code, can this be avoided by a parameter?
For example, my source code:
```python
FILE_SUFFIX_ICON = {
    MATLAB_SUFFIX: MATLAB_ICON,
    CPP_SUFFIX: CPP_ICON,
    SV_SUFFIX: RTL_ICON,
    'Default': FILE_ICON
}
```
After using format:
```python
FILE_SUFFIX_ICON = {
    MATLAB_SUFFIX: MATLAB_ICON,
    CPP_SUFFIX: CPP_ICON,
    SV_SUFFIX: RTL_ICON,
    'Default': FILE_ICON,
}
```


---

_Comment by @tjkuson on 2024-01-04 09:18_

The Ruff formatter tries to follow the [Black code style](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html), which adds a trailing comma. A configuration option has not been added to Ruff to change that behaviour (it would have to be a new feature).

Related discussion: #9350

---

_Comment by @17Reset on 2024-01-04 14:27_

Thanks, hope to add this feature option.

---

_Label `configuration` added by @charliermarsh on 2024-01-04 19:44_

---

_Label `formatter` added by @charliermarsh on 2024-01-04 19:44_

---

_Renamed from "Is the comma at the end of the list required?" to "Add support for optional trailing comma" by @charliermarsh on 2024-01-04 19:44_

---

_Label `configuration` removed by @charliermarsh on 2024-01-04 19:44_

---

_Label `needs-decision` added by @charliermarsh on 2024-01-04 19:44_

---

_Label `needs-decision` removed by @MichaReiser on 2024-01-08 08:14_

---

_Label `style` added by @MichaReiser on 2024-01-08 08:14_

---

_Comment by @MichaReiser on 2024-01-08 08:17_

There's no such option today. Would you mind explaining your motivation for omitting the trailing comma? 

Ruff supports the `--skip-magic-trailing-comma` option. It doesn't configure whether the trailing comma should be added, but it removes the trailing comma (and flattens the entries in the above example), if they all fit on a single line, e.g. after deleting or changing an entry.

---

_Comment by @heiner on 2024-07-01 21:36_

Here's a reason this might be useful: It turns `ruff format` into a quite decent `json` formatting engine. E.g.,

```
cat file.json | ruff format - --isolated --config indent-width=2
```

However, sadly trailing commas aren't legal JSON.

---

_Comment by @MichaReiser on 2024-07-02 05:49_

You may want to give [Biome](https://biomejs.dev/) a try if you are looking for a Rust based JSON formatter.

---

_Comment by @MichaReiser on 2024-07-02 05:51_

I'm closing this as not planned. Feel free to open a new issue to discuss the style with an outline for the use case and benefit of omitting trailing commas.

---

_Closed by @MichaReiser on 2024-07-02 05:51_

---

_Comment by @17Reset on 2024-07-02 08:11_

It's not a popular style for trailing commas to appear after line breaks in Python lists, is it?

---

_Comment by @heiner on 2024-07-02 13:37_

> You may want to give [Biome](https://biomejs.dev/) a try if you are looking for a Rust based JSON formatter.

That's less useful as a quick-and-dirty way to format JSON _within Python code_, where the existence of `ruff` is more or less guaranteed.

Still, ack'd on this being a bit fringe.

---

_Comment by @mshahbazi on 2024-08-28 06:31_

The issue arises when the formatter adds a trailing comma, which prevents the code from being one-liner, when shortened, due to the presence of the comma, unless the trailing comma is removed manually.

---

_Comment by @nyssance on 2024-10-14 14:12_

Black style for trailing comma is stupid.

---

_Comment by @dberardo-com on 2025-02-06 11:45_

i believe trailing commas dont work in python2.7, so it makes sense to make them optional

---

_Comment by @webknjaz on 2025-04-23 14:32_

I've found that a workaround is a double-pass of `ruff format` with an `add-trailing-comma` in-between: https://github.com/ansible/awx-plugins/blob/002bdc8/.pre-commit-config.yaml#L43-L58.

---
