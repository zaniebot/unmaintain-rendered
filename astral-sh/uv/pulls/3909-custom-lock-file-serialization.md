```yaml
number: 3909
title: Custom lock-file serialization
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: lock-format
created_at: 2024-05-29T15:28:18Z
updated_at: 2024-05-30T19:08:30Z
url: https://github.com/astral-sh/uv/pull/3909
synced_at: 2026-01-10T13:59:34Z
```

# Custom lock-file serialization

---

_Pull request opened by @ibraheemdev on 2024-05-29 15:28_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR changes the lock-file format to use inline tables for wheels and source distributions, which currently use separate tables that make the file harder to follow.

```diff
[[distribution]]
name = "typing-extensions"
version = "4.10.0"
source = "registry+https://pypi.org/simple"

- [distribution.sdist]
- url = "https://files.pythonhosted.org/packages/16/3a/0d26ce356c7465a19c9ea8814b960f8a36c3b0d07c323176620b7b483e44/typing_extensions-4.10.0.tar.gz"
- hash = "sha256:b0abd7c89e8fb96f98db18d86106ff1d90ab692004eb746cf6eda2682f91b3cb"
- size = 77558
-
- [[distribution.wheel]]
- url = "https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl"
- hash = "sha256:69b1a937c3a517342112fb4c6df7e72fc39a38e7891a5730ed4985b5214b5475"
- size = 33926

+ sdist = { url = "https://files.pythonhosted.org/packages/16/3a/0d26ce356c7465a19c9ea8814b960f8a36c3b0d07c323176620b7b483e44/typing_extensions-4.10.0.tar.gz", hash = "sha256:b0abd7c89e8fb96f98db18d86106ff1d90ab692004eb746cf6eda2682f91b3cb", size = 77558 }
+ wheel = [{ url = "https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl", hash = "sha256:69b1a937c3a517342112fb4c6df7e72fc39a38e7891a5730ed4985b5214b5475", size = 33926 }]
```

The downside is that the inline-tables end up quite long and TOML doesn't support line breaks in inline tables, yet.

Part of https://github.com/astral-sh/uv/issues/3611.


---

_Review requested from @BurntSushi by @ibraheemdev on 2024-05-29 15:28_

---

_@BurntSushi reviewed on 2024-05-30 13:33_

Nice!

I think the main question I have here is whether line breaks are inserted if there is more than one entry in the array. I think all the tests/examples currently only have one entry in these arrays, so it's hard to tell. I know line breaks can't be inserted in the inline tables themselves, but they can be inserted between the inline tables within the array. If there are no line breaks then we probably can't use this approach as the lines would get absurdly long.

I like the `to_toml` approach here. :)

---

_@BurntSushi approved on 2024-05-30 18:39_

Still makes for very long lines, but I feel like this is an improvement on the status quo.

The other possibility is to keep the table arrays (not inline tables) and just indent them.

---

_Comment by @codspeed-hq[bot] on 2024-05-30 18:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:lock-format)

### Merging #3909 will **not alter performance**

<sub>Comparing <code>ibraheemdev:lock-format</code> (8c570f1) with <code>main</code> (d3b7d80)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Merged by @ibraheemdev on 2024-05-30 19:08_

---

_Closed by @ibraheemdev on 2024-05-30 19:08_

---
