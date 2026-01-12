```yaml
number: 4357
title: "Improve documentation for `args_override_self`"
type: issue
state: closed
author: tmccombs
labels: []
assignees: []
created_at: 2022-10-08T08:34:35Z
updated_at: 2022-10-09T01:19:30Z
url: https://github.com/clap-rs/clap/issues/4357
synced_at: 2026-01-12T16:14:15Z
```

# Improve documentation for `args_override_self`

---

_@tmccombs_

The documentation for `args_override_self` states:

> Specifies that all arguments override themselves.

Which would seem to imply that it makes it so that args with the `Append` action  would only respect the last option given. But that doesn't appear to be the case (thankfully). This should probably be more specific that it applies to SetTrue, SetFalse, and Set. (how does it impact Count?).

Relatedly, the documentation for `override_with` still says:

> NOTE: All arguments implicitly override themselves.

But that is no longer true by default right?

---

_Closed by @epage on 2022-10-09 01:18_

---

_Comment by @epage on 2022-10-09 01:19_

Thanks for reporting this! I don't think clap was ever very clear on that but I've tried to make the distinction a bit clearer now.

I've also fixed that the comment about the default; missed that when doing the last minute behavior switch before clap 4

---
