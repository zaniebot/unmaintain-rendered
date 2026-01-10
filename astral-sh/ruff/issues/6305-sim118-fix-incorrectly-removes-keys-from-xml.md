---
number: 6305
title: "`SIM118` fix incorrectly removes `.keys()` from `xml.etree.ElementTree` object"
type: issue
state: closed
author: bouwew
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-08-03T14:54:17Z
updated_at: 2023-12-07T14:48:56Z
url: https://github.com/astral-sh/ruff/issues/6305
synced_at: 2026-01-10T01:22:45Z
---

# `SIM118` fix incorrectly removes `.keys()` from `xml.etree.ElementTree` object

---

_Issue opened by @bouwew on 2023-08-03 14:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

As defined here: https://docs.python.org/3/library/xml.etree.elementtree.html the `.keys()` element object is a valid one and differs from the dict `.keys()` object.


---

_Comment by @zanieb on 2023-08-03 14:56_

Hey @bouwew could you please include a code example as well as the rule code that's removing the `keys()` call?

---

_Comment by @bouwew on 2023-08-03 14:57_

```
    def _preset(self, loc_id: str) -> str | None:
        """Helper-function for smile.py: device_data_climate().

        Collect the active preset based on Location ID.
        """
        locator = "./rule[active='true']/directives/when/then"
        if (
            active_rule := self._domain_objects.find(locator)
        ) is None or "icon" not in active_rule.keys():  # noqa: SIM118
            return None
        return str(active_rule.attrib["icon"])
```

`self._domain_objects` contains xml-data. It's typed as `etree`, from `from defusedxml import ElementTree as etree`

---

_Comment by @charliermarsh on 2023-08-03 15:01_

IMO this is unavoidable without incorporating a full type inference engine. It will likely be a known bug for a while. Adding fix levels will help, since this fix is already marked as possibly-unsafe.

---

_Label `bug` added by @charliermarsh on 2023-08-03 15:02_

---

_Label `type-inference` added by @charliermarsh on 2023-08-03 15:02_

---

_Comment by @bouwew on 2023-08-03 15:03_

I think I could use `.items()` as a work-around I've found in the meantime, so not a big problem :)

Tnx for the quick responses!

---

_Renamed from "BUG: ruff removes the .keys() extention incorrectly when used with xml.etree.ElementTree" to "`SIM118` fix incorrectly removes `.keys()` from `xml.etree.ElementTree` object" by @zanieb on 2023-08-03 15:15_

---

_Comment by @charliermarsh on 2023-12-07 14:48_

We now mark this as unsafe if the object isn't a dictionary.

---

_Closed by @charliermarsh on 2023-12-07 14:48_

---
