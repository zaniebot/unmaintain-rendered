```yaml
number: 10534
title: "Show 'closest' tag in tag incompatibility error messages"
type: pull_request
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
base: main
head: charlie/closest
created_at: 2025-01-12T02:55:33Z
updated_at: 2025-01-14T03:33:58Z
url: https://github.com/astral-sh/uv/pull/10534
synced_at: 2026-01-10T11:44:56Z
```

# Show 'closest' tag in tag incompatibility error messages

---

_Pull request opened by @charliermarsh on 2025-01-12 02:55_

## Summary

Here's an example from `torch` on my M3:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of torch are available:
          torch==1.0.0
          torch==1.0.1
          torch==1.1.0
          torch==1.2.0
          torch==1.3.0
          torch==1.3.1
          torch==1.4.0
          torch==1.5.0
          torch==1.5.1
          torch==1.6.0
          torch==1.7.0
          torch==1.7.1
          torch==1.8.0
          torch==1.8.1
          torch==1.9.0
          torch==1.9.1
          torch==1.10.0
          torch==1.10.1
          torch==1.10.2
          torch==1.11.0
          torch==1.12.0
          torch==1.12.1
          torch==1.13.0
          torch==1.13.1
          torch==2.0.0
          torch==2.0.1
          torch==2.1.0
          torch==2.1.1
          torch==2.1.2
          torch==2.2.0
          torch==2.2.1
          torch==2.2.2
          torch==2.3.0
          torch==2.3.1
          torch==2.4.0
          torch==2.4.1
          torch==2.5.0
          torch==2.5.1
      and torch<=2.4.1 has no wheels with a matching Python ABI tag (e.g., `cp313`), we can conclude that torch<=2.4.1 cannot be used.
      And because torch>=2.5.0 has no wheels with a matching platform tag (e.g., `macosx_14_0_arm64`) and you require torch, we can conclude that your requirements are unsatisfiable.

      hint: Wheels are available for `torch` (v2.4.1) with the following ABI tags: `cp38`, `cp39`, `cp310`, `cp311`, `cp312`. The closest match is `cp312-none-macosx_11_0_arm64`.

      hint: Wheels are available for `torch` (v2.5.1) on the following platform: `manylinux1_x86_64`. The closest match is `cp313-cp313-manylinux1_x86_64`.
```


---

_Label `error messages` added by @charliermarsh on 2025-01-12 02:55_

---

_@charliermarsh reviewed on 2025-01-12 02:56_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:584 on 2025-01-12 02:56_

This is really, really inefficient... It's O(M*N) where M is the number of compatible tags and N is the number of wheels. But like... it will only run once, in the error reporting, so I think it's kind of fine?

---

_Review requested from @zanieb by @charliermarsh on 2025-01-13 22:46_

---

_Review requested from @konstin by @charliermarsh on 2025-01-13 22:46_

---

_@charliermarsh reviewed on 2025-01-13 22:46_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1639 on 2025-01-13 22:46_

I think @konsti had a nice suggestion, to say something like: "A wheel exists for a different architecture" (https://github.com/astral-sh/uv/issues/2777#issuecomment-2588264790)

---

_@zanieb reviewed on 2025-01-13 23:07_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-13 23:07_

I'm not sure these help.

In this example, I get confused by the additional tags (e.g., `cp310`). Maybe it'd be helpful as "The closest match to your platform (e.g., ...) is ...", but I'm not sure I follow the user experience this solves.

---

_@zanieb reviewed on 2025-01-13 23:08_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-13 23:08_

Separately, how is `cp310-cp310-win_amd64` the closest match to `manylinux_2_17_x86_64`?

---

_@zanieb reviewed on 2025-01-13 23:08_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:4135 on 2025-01-13 23:08_

I imagine one wheel platform is rare, but should we show a closest match in this case? 

---

_@charliermarsh reviewed on 2025-01-13 23:26_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-13 23:26_

It should probably be `cp310-cp310-manylinux_2_27_x86_64 `. You don't find that helpful?

---

_@zanieb reviewed on 2025-01-13 23:32_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13952 on 2025-01-13 23:32_

I don't think so. Maybe if I _also_ had a Python compatibility error? I'm just not sure what I could do with that information, and a quick scan of the available tags is already showing where I've gone wrong.

What does knowing the closest tag change? Does it make a particular action clearer?

---

_Closed by @charliermarsh on 2025-01-14 03:33_

---

_Comment by @charliermarsh on 2025-01-14 03:33_

Just gonna close this. Other more impactful things to improve in these errors.

---
