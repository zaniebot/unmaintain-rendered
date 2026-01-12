```yaml
number: 884
title: No solution error any-of list is inverted
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2024-01-11T12:24:19Z
updated_at: 2024-02-02T18:00:27Z
url: https://github.com/astral-sh/uv/issues/884
synced_at: 2026-01-12T15:58:24Z
```

# No solution error any-of list is inverted

---

_@konstin_

When resolving `recommenders>0.7` on the incompatible python 3.12, we get a list of holes instead of list of possible versions. The list is misleading since we only need to know that there are not other versions in `recommenders>0.7,<1.1.0` except `recommenders==1.0.0` (we don't care about higher versions such as 1.1.1) to conclude that `recommenders>0.7,<1.1.0` cannot be used.

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of Python that satisfy Python>=3.6,<3.10 and recommenders==1.0.0 depends on Python>=3.6,<3.9, we
      can conclude that recommenders==1.0.0 cannot be used.
      And because there are no versions of recommenders that satisfy any of:
          recommenders>0.7,<1.0.0
          recommenders>1.0.0,<1.1.0
          recommenders>1.1.0,<1.1.1
          recommenders>1.1.1
      we can conclude that recommenders>0.7,<1.1.0 cannot be used. (1)

      Because there are no versions of Python that satisfy Python>=3.6,<3.10 and recommenders>=1.1.0 depends on Python>=3.6,<3.10,
      we can conclude that recommenders>=1.1.0 cannot be used.
      And because we know from (1) that recommenders>0.7,<1.1.0 cannot be used, we can conclude that recommenders>0.7 cannot be
      used.
      And because root depends on recommenders>0.7 we can conclude that the requirements are unsatisfiable.
```

---

_Label `error messages` added by @konstin on 2024-01-11 12:24_

---

_Comment by @konstin on 2024-01-11 12:54_

Additional example without the python version in it: Here the problem is that `pytorch-lightning>1.8.3.post2`  requires `torch>=1.9.0`, while root requires `torch==1.8.0`

```
cargo run -q -- pip-install "torch==1.8.0" "deepblast==1.0.2" "pytorch-lightning>1.8.3.post2"
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of pytorch-lightning that satisfy any of:
          pytorch-lightning>1.8.6,<1.9.0
          pytorch-lightning>1.9.5,<2.0.0
          pytorch-lightning>2.0.9.post0,<2.1.0
      and pytorch-lightning>=1.8.4,<=1.8.6 depends on torch>=1.9.0, we can conclude that any of:
          pytorch-lightning>1.8.3.post2,<1.9.0
          pytorch-lightning>1.9.5,<2.0.0
          pytorch-lightning>2.0.9.post0,<2.1.0
      depends on torch>=1.9.0.
      And because pytorch-lightning>=1.9.0,<=1.9.5 depends on torch>=1.10.0 we can conclude that any of:
          pytorch-lightning>1.8.3.post2,<2.0.0
          pytorch-lightning>2.0.9.post0,<2.1.0
      depends on torch>=1.9.0.
      And because pytorch-lightning>=2.0.0,<=2.0.9.post0 depends on torch>=1.11.0 and pytorch-lightning>=2.1.0 depends on
      torch>=1.12.0, we can conclude that pytorch-lightning>1.8.3.post2 depends on torch>=1.9.0.
      And because root depends on torch==1.8.0 and root depends on pytorch-lightning>1.8.3.post2, we can conclude that the
      requirements are unsatisfiable.

      hint: Pre-releases are available for pytorch-lightning in the requested range (e.g., 2.1.0rc1), but pre-releases weren't
      enabled (try: `--prerelease=allow`)
```

---

_Comment by @zanieb on 2024-01-11 14:48_

Thanks! This one is on my radar but the examples are helpful.

---

_Comment by @konstin on 2024-01-11 15:27_

Here's a recent run of pypi top 10k most dependents with builds reduced to errors, it has a bunch of examples: https://gist.github.com/konstin/b32a9cf1a07e061ac0236776eab34f3c . I'd expect that those a underrepresented in our scenarios since they have less versions and lack the progression of increasing requirements over releases.

---

_Closed by @zanieb on 2024-02-02 18:00_

---
