```yaml
number: 13083
title: Add anchor tags to README headers
type: pull_request
state: merged
author: calumy
labels:
  - documentation
assignees: []
merged: true
base: main
head: add-links-to-pypi-docs
created_at: 2024-08-23T16:34:19Z
updated_at: 2024-08-26T10:47:44Z
url: https://github.com/astral-sh/ruff/pull/13083
synced_at: 2026-01-12T15:55:43Z
```

# Add anchor tags to README headers

---

_@calumy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This pull request adds anchor tags to the elements referenced in the table of contents section of the readme used on [PyPI](https://pypi.org/project/ruff/) as an attempt to fix #7257. This update follows [this suggestion](https://github.com/pypa/readme_renderer/issues/169#issuecomment-808577486) to add anchor tags (with no spaces) after the title that is to be linked to.

## Test Plan

<!-- How was it tested? -->
- This has been tested on GitHub to check that the additional tags do not interfere with how the read me is rendered; see: https://github.com/calumy/ruff/blob/add-links-to-pypi-docs/README.md
- MK docs were generated using the `generate_mkdocs.py` script; however  as the added tags are beyond the comment `<!-- End section: Overview -->`, they are excluded so will not change how the docs are rendered.
- I was unable to verify how PyPI renders this change, any suggestions would be appreciated and I can follow up on this. Hopefully, the four thumbs up/heart on [this comment](https://github.com/pypa/readme_renderer/issues/169#issuecomment-808577486) and [this suggestion](https://github.com/pypa/readme_renderer/issues/169#issuecomment-1765616890)  all suggest that this approach should work.


---

_@dhruvmanila approved on 2024-08-26 07:11_

Thanks!

I think it should work on PyPI as well based on https://github.com/databricks/databricks-sdk-py/blob/main/README.md?plain=1 and https://pypi.org/project/databricks-sdk.

---

_Renamed from "Add anchor tags to readme links" to "Add anchor tags to README headers" by @dhruvmanila on 2024-08-26 07:11_

---

_Label `documentation` added by @dhruvmanila on 2024-08-26 07:11_

---

_Merged by @dhruvmanila on 2024-08-26 07:12_

---

_Closed by @dhruvmanila on 2024-08-26 07:12_

---

_Comment by @calumy on 2024-08-26 10:47_

> Thanks!
> 
> 
> 
> I think it should work on PyPI as well based on https://github.com/databricks/databricks-sdk-py/blob/main/README.md?plain=1 and https://pypi.org/project/databricks-sdk.

Thanks for finding an example, I couldn't find one at the time of writing the PR. 



---
