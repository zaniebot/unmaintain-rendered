---
number: 8021
title: "The formatter in ruff 0.1.0 ignores `# fmt: off` pragma inside of an assert statement"
type: issue
state: closed
author: blakeNaccarato
labels:
  - formatter
assignees: []
created_at: 2023-10-17T17:46:06Z
updated_at: 2023-10-17T23:57:39Z
url: https://github.com/astral-sh/ruff/issues/8021
synced_at: 2026-01-07T13:12:15-06:00
---

# The formatter in ruff 0.1.0 ignores `# fmt: off` pragma inside of an assert statement

---

_Issue opened by @blakeNaccarato on 2023-10-17 17:46_

The formatter in ruff 0.1.0 ignores `# fmt: off` pragma inside of an assert statement. Ruff respects the pragma when running `ruff format --isolated test.py` when `test.py` looks like this:

```Python
# fmt: off
assert (
    (1111111111111111111111111111111111111111111111111111111111111111111111111111111, 2,)
)
```

But Ruff does not respect the pragma when `test.py` looks like this:

```Python
assert (
    # fmt: off
    (1111111111111111111111111111111111111111111111111111111111111111111111111111111, 2,)
)
```

Promoting the `# fmt: off` pragma outside of the assert statement is a workaround for now. This was a minimal example, but you could see how it affects a real test like this:

```Python
def test_example():
    """Example test that asserts something about a specially-formatted array."""
    assert np.allclose(
        ...,
        np.array(
            [
                # fmt: off
                105.        , 106.02040672, 107.04081917, 108.06122589,
                109.08163071, 110.10204124, 111.12244987, 112.14286041,
                113.16326523, 114.18367195, 115.2040844 , 116.22449112,
                165.3471179 , 167.69404545, 170.10337013, 172.57614547
            ]
        ),
    )
```

---

_Label `formatter` added by @zanieb on 2023-10-17 17:48_

---

_Comment by @MichaReiser on 2023-10-17 23:12_

Hy @blakeNaccarato 

Thanks for raising this. Ruff only supports statement-level suppression comments. This doesn't seem to be mentioned in the alpha documentation but we have a dedicated section in the soon published [formatter documentation](https://github.com/astral-sh/ruff/blob/charlie/formatter-docs/docs/formatter.md#format-suppression) 

---

_Comment by @blakeNaccarato on 2023-10-17 23:41_

Ah, thanks for the heads up. I'm sure that this decision simplifies the bug surface area significantly, so I can understand why it wouldn't be supported.

As the tool develops further, it may be helpful to have some parallel ruff linter category that warns on possible `# fmt: off` sharp edges. The message could be e.g. `Unsupported fmt pragma usage: Ruff formatter only supports statement-level suppression` with some heuristic that kinda/sorta catches attempts like this (not robust enough to catch all cases, otherwise it would have just been a feature).

I'm gonna close this as wont-fix then if that's alright.

---

_Closed by @blakeNaccarato on 2023-10-17 23:41_

---

_Comment by @MichaReiser on 2023-10-17 23:57_

> As the tool develops further, it may be helpful to have some parallel ruff linter category that warns on possible # fmt: off sharp edges. The message could be e.g. Unsupported fmt pragma usage: Ruff formatter only supports statement-level suppression with some heuristic that kinda/sorta catches attempts like this (not robust enough to catch all cases, otherwise it would have just been a feature).

yes, this is a great idea and something we consider implementing for the stable release. 

---
