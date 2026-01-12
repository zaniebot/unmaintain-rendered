```yaml
number: 8157
title: "\"Invalid `# ruff: noqa`\" warnings should be an error"
type: issue
state: open
author: DetachHead
labels:
  - suppression
  - wish
assignees: []
created_at: 2023-10-24T06:09:33Z
updated_at: 2025-05-09T06:06:33Z
url: https://github.com/astral-sh/ruff/issues/8157
synced_at: 2026-01-12T15:54:47Z
```

# "Invalid `# ruff: noqa`" warnings should be an error

---

_@DetachHead_

```py
# ruff: noqa: asdfas
```
```
>ruff asdf.py
warning: Invalid `# ruff: noqa` directive at asdf.py:1: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

currently there seems to be no way to make ruff fail on invalid noqa comments. i think this should be a normal rule that causes ruff to fail, not a warning.

---

_Comment by @zanieb on 2023-10-24 16:45_

It does seems reasonable to include a rule code for this, but given that we cannot parse the noqa directive it could be troublesome as you won't be able to ignore problems.

Separately from whether or not this is something we want, I think it'd be a little tricky to implement.

---

_Label `noqa` added by @zanieb on 2023-10-24 16:46_

---

_Label `needs-decision` added by @zanieb on 2023-10-24 16:46_

---

_Comment by @andy-maier on 2024-04-09 07:14_

I have this warning too, but in my case it was caused by the use of `noga: 501` which apparently works for flake8 but causes the warning for ruff. We addressed that by changing that to `noga: E501`.

However, since flake8 supports the notation `noga: 501`, it would probably be good if ruff did that, too.

Apart from that, I tend to agree that invalid noqa statements should be errors.

---

_Comment by @DetachHead on 2025-02-06 02:15_

related: #15682

i think when i raised this only `# ruff: noqa:` comments had this issue, but now the same thing happens with `# noqa:` comments.

i think this warning can probably just be removed once those issues with `RUF100` are fixed

---

_Comment by @KotlinIsland on 2025-02-06 02:16_

why don't these warnings show in the lsp? i don't think anyone would ever even notice that they exist otherwise

---

_Comment by @MichaReiser on 2025-02-06 07:50_

It's because they're not using our regular diagnostic system (because all our diagnostics are errors at the moment). A first step is to support warning level diagnostics (@BurntSushi is working on this). A next step is to change the rule to generate diagnostics but that's probably a bit more involved and the final step would be to allow making this an error instead.

---

_Comment by @KotlinIsland on 2025-02-07 03:08_

if a warning is emitted, will it still cause the ci to fail? I can't really see any use-case for it otherwise 

---

_Comment by @MichaReiser on 2025-02-07 07:40_

No, CI won't fail which is the same as for other warnings emitted by Ruff, e.g. warnings about incorrect or conflicting configurations, failures to read the cache, etc. This behavior is intentional (in the absence of an explicit warning severity and a `--error-on-warning` flag). 

My assessment here: Invalid noqa comments should be its own rule that uses the warning severity. So this is blocked on adding support for warning severities (which we support in Red Knot and where we have a dedicate rule just for this)



---

_Label `needs-decision` removed by @MichaReiser on 2025-02-07 07:41_

---

_Label `wish` added by @MichaReiser on 2025-02-07 07:41_

---

_Comment by @MichaReiser on 2025-05-09 06:06_

One thing we could try is to see if we can setup a custom tracing subscriber that records if it has seen any warning log message, and, if so, return with an error exit code if `error-on-warning` is enabled in the configuration.

---
