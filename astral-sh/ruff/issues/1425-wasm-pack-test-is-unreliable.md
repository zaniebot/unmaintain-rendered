---
number: 1425
title: "`wasm-pack` test is unreliable"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2022-12-28T15:10:39Z
updated_at: 2024-02-23T22:01:36Z
url: https://github.com/astral-sh/ruff/issues/1425
synced_at: 2026-01-10T01:22:39Z
---

# `wasm-pack` test is unreliable

---

_Issue opened by @charliermarsh on 2022-12-28 15:10_

See: https://github.com/charliermarsh/ruff/actions/runs/3794761527/jobs/6453331265.

---

_Comment by @squiddy on 2022-12-28 15:14_

What I don't understand - and I noticed that in multiple runs - is why the operation is being canceled. Many times the tests succeeded. 

```
running 3 tests

test ruff::lib_wasm::test::empty_config ... ok
test ruff::lib_wasm::test::partial_config ... ok
test ruff::lib_wasm::test::partial_nested_config ... ok

test result: ok. 3 passed; 0 failed; 0 ignored

Error: The operation was canceled.
```

---

_Comment by @charliermarsh on 2022-12-28 15:15_

Yeah, trying to find any relevant issues etc. but not seeing anything.

---

_Comment by @charliermarsh on 2022-12-28 15:24_

I don't know if this is just a symptom of a different failure, but when it fails in this way, we never see the other test suites skip execution:

<img width="566" alt="Screen Shot 2022-12-28 at 10 24 08 AM" src="https://user-images.githubusercontent.com/1309177/209834183-e99e339f-a0d3-4714-9807-1569211db022.png">
<img width="1108" alt="Screen Shot 2022-12-28 at 10 24 12 AM" src="https://user-images.githubusercontent.com/1309177/209834186-2a537a15-141b-44b4-97d7-dd1f981eadad.png">


---

_Comment by @charliermarsh on 2022-12-28 15:28_

I'll try setting `--lib` for now... I dunno.

---

_Comment by @squiddy on 2022-12-28 15:34_

I'm also seeing this.. which isn't that helpful. I was thinking that maybe it's just being canceled because a new workflow was scheduled, but that doesn't seem to be the case.
I would also kinda rule out memory issues, we're not even running headless browsers for testing..

```
2022-12-28T15:22:07.1731677Z running 3 tests
2022-12-28T15:22:07.1831067Z 
2022-12-28T15:22:07.2165473Z test ruff::lib_wasm::test::empty_config ... ok
2022-12-28T15:22:07.2217990Z test ruff::lib_wasm::test::partial_config ... ok
2022-12-28T15:22:07.2433964Z test ruff::lib_wasm::test::partial_nested_config ... ok
2022-12-28T15:22:07.2457576Z 
2022-12-28T15:22:07.2459785Z test result: ok. 3 passed; 0 failed; 0 ignored
2022-12-28T15:22:07.2460027Z 
2022-12-28T15:22:41.4902290Z ##[error]Process completed with exit code 143.
2022-12-28T15:22:41.4984194Z ##[error]The runner has received a shutdown signal. This can happen when the runner service is stopped, or a manually started runner is canceled.
```

---

_Comment by @squiddy on 2022-12-28 15:36_

Well, actually.. when you start searching for the "shutdown signal" thing, numerous issues popup about people's jobs being canceled. Most often due to resource limits.

For example: https://github.com/actions/runner-images/issues/6680
One response: https://github.com/actions/runner-images/issues/6680#issuecomment-1335778010

---

_Referenced in [astral-sh/ruff#1427](../../astral-sh/ruff/pulls/1427.md) on 2022-12-28 17:27_

---

_Label `bug` added by @charliermarsh on 2022-12-28 22:27_

---

_Label `internal` added by @charliermarsh on 2022-12-28 22:27_

---

_Label `bug` removed by @charliermarsh on 2023-02-17 23:43_

---

_Comment by @MichaReiser on 2024-02-23 22:01_

I don't remember having seen this in months. 

---

_Closed by @MichaReiser on 2024-02-23 22:01_

---
