```yaml
number: 14069
title: Retry streaming distribution and Python downloads if failing mid-stream
type: issue
state: open
author: konstin
labels:
  - enhancement
  - network
assignees: []
created_at: 2025-06-16T08:33:07Z
updated_at: 2025-07-01T16:55:04Z
url: https://github.com/astral-sh/uv/issues/14069
synced_at: 2026-01-12T16:01:42Z
```

# Retry streaming distribution and Python downloads if failing mid-stream

---

_@konstin_

We have retry support if there is an IO or status code error in the initial request (https://github.com/astral-sh/uv/pull/13897). If the initial response succeeded (usually with status code 200) and we convert the response to a streaming download, the streaming download-and-unpack is not retried, and we lose the reliable retry information.

Broken down into items:
- [ ] Show previous retries when a distribution download fails mid-streaming
- [ ] Perform retries when a distribution download fails mid-streaming
- [x] Show previous retries when a Python download fails mid-streaming (https://github.com/astral-sh/uv/pull/14378)
- [ ] Perform retries when a Python download fails mid-streaming
    - (Jack: It's not clear that this is missing? [The only cases I've seen fail to retry are synthetic.](https://github.com/astral-sh/uv/issues/14171#issuecomment-3014580701))



---

_Label `enhancement` added by @konstin on 2025-06-16 08:33_

---

_Label `network` added by @konstin on 2025-06-16 08:33_

---

_Comment by @oconnor663 on 2025-06-30 18:48_

In playing with https://github.com/astral-sh/uv/issues/14171, I'm noticing that we retry certain errors more than I think(?) we intended. For example if I have my local Python mirror force a ConnectionReset error during the "streaming phase" of a download, we retry 3 times (for a total of 4 attempts), but if I force the same error at the very start of the GET request, I see 16 attempts. My theory is that we have nested retrying going on, with the inner loop done (indirectly) by `apply_middleware`: https://github.com/astral-sh/uv/blob/1c7c174bc80e9209bb52f975743c0cf55bc78e6d/crates/uv-client/src/base_client.rs#L412-L420

and the outer loop done (explicitly) by `fetch_with_retry`: https://github.com/astral-sh/uv/blob/1c7c174bc80e9209bb52f975743c0cf55bc78e6d/crates/uv-python/src/downloads.rs#L719-L730

Is that correct? Is it expected, or maybe a bug?

---

_Comment by @zanieb on 2025-06-30 18:51_

Sounds plausible that is unintentional

---

_Comment by @konstin on 2025-06-30 19:57_

There's definitely retry nesting, mainly due to io errors vs. status code errors, where some of the nesting behavior is unintentional.

---

_Comment by @oconnor663 on 2025-06-30 20:44_

@konstin do you think it's a good idea to "fix" some of these "excessive" retries as part of getting the logging correct, or maybe better to just get the logging correct but keep the retry behavior itself unchanged? (Like I can see an argument that if we want 3 retries, but you get 3 distinct errors with different causes, it's not actually a bad thing to keep retrying until you actually get the same error 3 times.)

---

_Comment by @konstin on 2025-07-01 10:57_

Having a consistent number of retries is preferable, though excessive retries are not really a problem. I'm more worried that the logic is overly complex and thereby faulty through retrying on slightly different conditions in different layers.

---
