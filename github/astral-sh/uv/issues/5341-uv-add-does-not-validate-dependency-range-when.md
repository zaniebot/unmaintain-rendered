---
number: 5341
title: "`uv add` does not validate dependency range when source is present"
type: issue
state: open
author: zanieb
labels: []
assignees: []
created_at: 2024-07-23T15:56:26Z
updated_at: 2024-08-20T18:22:20Z
url: https://github.com/astral-sh/uv/issues/5341
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv add` does not validate dependency range when source is present

---

_Issue opened by @zanieb on 2024-07-23 15:56_

e.g. the following fails

```
❯ uv add 'httpx>9999'
warning: `uv add` is experimental and may change without warning
warning: No `requires-python` field found in the workspace. Defaulting to `>=3.12`.
error: Because only httpx<=9999 is available and example==0.1.0 depends on httpx>9999, we can conclude that example==0.1.0 cannot be used.
And because only example==0.1.0 is available and you require example, we can conclude that the requirements are unsatisfiable.
```

but this does not

```
❯ uv add git+https://github.com/encode/httpx
warning: `uv add` is experimental and may change without warning
 Updated https://github.com/encode/httpx (beb501f)
warning: No `requires-python` field found in the workspace. Defaulting to `>=3.12`.
⠙ Resolving dependencies...                                                                                                                              warning: `uv.sources` is experimental and may change without warning
 Updated https://github.com/encode/httpx (beb501f)
Resolved 8 packages in 373ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 390ms
Uninstalled 1 package in 0.44ms
Installed 1 package in 0.92ms
 - example==0.1.0 (from file:///Users/zb/workspace/example)
 + example==0.1.0 (from file:///Users/zb/workspace/example)

❯ uv add 'httpx>9999'
warning: `uv add` is experimental and may change without warning
warning: No `requires-python` field found in the workspace. Defaulting to `>=3.12`.
⠙ Resolving dependencies...                                                                                                                              warning: `uv.sources` is experimental and may change without warning
Resolved 8 packages in 29ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 429ms
Uninstalled 1 package in 0.34ms
Installed 1 package in 0.89ms
 - example==0.1.0 (from file:///Users/zb/workspace/example)
 + example==0.1.0 (from file:///Users/zb/workspace/example)
```

I'm guessing this is somewhat intentional, but seems like a footgun?

---

_Label `preview` added by @zanieb on 2024-07-23 15:56_

---

_Comment by @charliermarsh on 2024-07-23 15:57_

I think this is just https://github.com/astral-sh/uv/issues/4604.

---

_Comment by @charliermarsh on 2024-07-23 16:04_

We should probably (1) preserve the specifier, and (2) error if the specifier is a URL and sources is set. Can you merge with #4604 and #4603 if correct?

---

_Comment by @zanieb on 2024-07-23 16:09_

Thanks for the refs!

I think it's a little different than #4604, though the fix for #4604 may resolve this. #4604 / #4603 are checking for coherence between the resolved source and the PEP 508 dependency. Here, I'm complaining that the PEP 508 dependency is not validated _independently_ of the source. The resolution without sources needs to be valid to publish the package.

Do you think we should close in favor of those still?

---

_Comment by @charliermarsh on 2024-07-23 16:10_

I think https://github.com/astral-sh/uv/issues/4604 is the same, right? It's saying that we should validate that the source matches the PEP 508 dependency.

---

_Comment by @zanieb on 2024-07-23 16:12_

#4604 says "resolve the source, get its version, check if it is contained in the PEP 508 dependency"

Here, I say: "can we resolve the PEP 508 dependency without the source"

---

_Comment by @zanieb on 2024-07-23 16:13_

This seems like it would be a warning on add whereas #4604 would be an error?

---

_Comment by @charliermarsh on 2024-07-23 16:45_

Ok, we can just leave them both.

---

_Referenced in [astral-sh/uv#5383](../../astral-sh/uv/issues/5383.md) on 2024-07-24 09:02_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---
