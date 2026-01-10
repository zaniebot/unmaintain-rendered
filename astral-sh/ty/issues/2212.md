```yaml
number: 2212
title: "error message improvement: spell out MRO and other TLAs"
type: issue
state: closed
author: pjonsson
labels:
  - diagnostics
assignees: []
created_at: 2025-12-24T23:07:11Z
updated_at: 2025-12-25T10:26:46Z
url: https://github.com/astral-sh/ty/issues/2212
synced_at: 2026-01-10T01:56:41Z
```

# error message improvement: spell out MRO and other TLAs

---

_Issue opened by @pjonsson on 2025-12-24 23:07_

### Summary

I just installed ty 0.0.7 and took it for a spin and it has come a long way since I checked it out a couple of months ago, very impressive!

During my test drive, I managed to get this error message:
```
info: ty cannot resolve a consistent MRO for class `Name` due to this base
info: Only class objects or `Any` are supported as class bases
info: rule `unsupported-base` is enabled by default
```
and I had to look in the documentation (https://docs.astral.sh/ty/reference/rules/#inconsistent-mro) to understand what MRO stands for. I know what a method resolution order is, and even if I didn't, I could probably guess.

I'm not sure what the policy is, but I think ty should spell out words in error messages to the greatest extent possible, "MRO"/"OOB" aren't quicker to interpret for the reader than "method resolution order"/"out of bounds" and having all three words eliminates the need for guesswork when reading the error messages. I'm sure there are some exceptions where the abbreviation is more known than its meaning (like "CD" instead of "compact disc"), but my hunch is that those are not the norm.

### Version

ty 0.0.7

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-24 23:11_

---

_Comment by @AlexWaygood on 2025-12-24 23:12_

Thanks, this is great feedback! We can definitely improve the message here as per your suggestion 

---

_Closed by @AlexWaygood on 2025-12-25 10:26_

---
