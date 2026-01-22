```yaml
number: 2575
title: "Extreme slowdown when using types from `pulumi_cloudflare` (~12 min vs sub-second for `pulumi_aws`)"
type: issue
state: open
author: michal-stlv
labels:
  - performance
assignees: []
created_at: 2026-01-21T10:31:16Z
updated_at: 2026-01-22T00:24:50Z
url: https://github.com/astral-sh/ty/issues/2575
synced_at: 2026-01-22T01:09:06Z
```

# Extreme slowdown when using types from `pulumi_cloudflare` (~12 min vs sub-second for `pulumi_aws`)

---

_@michal-stlv_

### Summary

I'm experiencing a significant performance issue when type checking code that uses `pulumi_cloudflare`. 

Using any type from the package causes ty to take ~12 minutes, while just importing it (without using any types) is instant. The much larger `pulumi_aws` package works fine.

### Reproduction

Tested on ty 0.0.12, Python 3.12.

**Slow case - using a type from pulumi_cloudflare:**

```python
# slow_cf.py
import pulumi_cloudflare

record = pulumi_cloudflare.Record(
    "test",
    zone_id="xxx",
    name="test",
    type="A",
    content="1.2.3.4",
)
```

```
$ ty check slow_cf.py
All checks passed!
Took    12m29.81s
```

**Fast case - import only, no usage:**

```python
# fast_cf.py
import pulumi_cloudflare
```

```
$ ty check fast_cf.py
All checks passed!
Took    0.058s
```

**pulumi_aws works fine:**

```python
# aws_test.py
import pulumi_aws

bucket = pulumi_aws.s3.Bucket("test")
```

```
$ ty check aws_test.py
All checks passed!
Took    0.241s
```

### Environment

- ty: 0.0.12
- Python: 3.12
- pulumi_cloudflare: 6.12.0
- pulumi_aws: 7.16.0
- macOS Tahoe (M3 Max, 36GB)

### Observations

I looked into why this might be happening (though I could be wrong about the details):

- `pulumi_cloudflare` is smaller (42MB) than `pulumi_aws` (121MB), yet much slower
- `pulumi_cloudflare` appears to have a more monolithic structure - I noticed `_inputs.py` is quite large (~88K lines) compared to `pulumi_aws` which seems to split things across many files
- Both packages use `Input[T] = Union[T, Awaitable[T], Output[T]]` pattern with TypedDicts

I found that mypy has a similar issue documented in python/mypy#17231, where Pulumi's TypedDict + Union patterns cause performance problems. Pyright apparently handles this better. Not sure if the same root cause applies here.

This might be related to #240 (large union benchmarks), though I'm not certain.

### Workaround

Excluding the folder from ty checking works for now:

```toml
[tool.ty.src]
exclude = ["path/to/code/using/cloudflare/"]
```

### Questions

- Is this a known pattern that causes issues for ty?
- Any other workarounds besides excluding?
- Any plans for handling this?

Thanks for the amazing tool!

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---

_Label `performance` added by @AlexWaygood on 2026-01-21 10:52_

---

_Comment by @MichaReiser on 2026-01-21 13:37_

Thank you for reporting this. This is very slow! 

I didn't run the check command to completion, but ty spends almost all time in `ReachabilityConstraints::add_and_constraint` ([profile](https://share.firefox.dev/3LXRl27)) but it's not entirely clear to me which code is triggering this exponential blow up (CC: @dcreager in case you're aware of some blow up cases)

---

_Added to milestone `Stable` by @carljm on 2026-01-22 00:24_

---
