```yaml
number: 2693
title: Turn on PCRE2 automatically with a warning when look-around or backreferences are detected
type: issue
state: closed
author: futpib
labels:
  - invalid
assignees: []
created_at: 2023-12-20T18:51:34Z
updated_at: 2023-12-30T13:40:42Z
url: https://github.com/BurntSushi/ripgrep/issues/2693
synced_at: 2026-01-12T16:13:24Z
```

# Turn on PCRE2 automatically with a warning when look-around or backreferences are detected

---

_@futpib_

#### Describe your feature request

```bash
$ rg '\.invalidateQueries\((?!\[)'

regex parse error:
    (?:\.invalidateQueries\((?!\[))
                            ^^^
error: look-around, including look-ahead and look-behind, is not supported

Consider enabling PCRE2 with the --pcre2 flag, which can handle backreferences
and look-around.
```

ripgrep detects unsupported regex features and gives a good hint to enable pcre2, but it could instead print a warning, enable it automatically and continue with pcre2 enabled.

This would save the user some keystrokes for one-off searches where this warning can usually be ignored.

```bash
$ rg '\.invalidateQueries\((?!\[)'

regex parse error:
    (?:\.invalidateQueries\((?!\[))
                            ^^^
warning: look-around, including look-ahead and look-behind, is not supported by default

To suppress this warning consider enabling PCRE2 explicitly with the --pcre2 flag, which
can handle backreferences and look-around but has associated performance tradeoffs.
Consult the ripgrep manual for more info. Continuing with PCRE2 enabled for convenience.
```

ripgrep could also detect if running on a TTY or in a script and turn this warning into an error in scripts if PCRE2 performance concerns are worth it.

---

_Comment by @BurntSushi on 2023-12-20 19:19_

Could you please explain why `--engine auto` is insufficient for your use case?

---

_Comment by @futpib on 2023-12-20 20:12_

Thanks, I just did not know about it. It is sufficient paired with a config file. Now I only with it was the default.

---

_Closed by @futpib on 2023-12-20 20:12_

---

_Label `invalid` added by @BurntSushi on 2023-12-30 13:40_

---
