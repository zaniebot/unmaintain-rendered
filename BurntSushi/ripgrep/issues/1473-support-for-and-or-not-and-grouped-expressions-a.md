```yaml
number: 1473
title: "Support for `--and`, `--or`, `--not`, and grouped expressions a la Git Grep"
type: issue
state: closed
author: zachriggle
labels:
  - duplicate
assignees: []
created_at: 2020-01-28T00:13:04Z
updated_at: 2020-01-28T00:24:15Z
url: https://github.com/BurntSushi/ripgrep/issues/1473
synced_at: 2026-01-12T16:13:23Z
```

# Support for `--and`, `--or`, `--not`, and grouped expressions a la Git Grep

---

_@zachriggle_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`brew install ripgrep --HEAD`

#### What operating system are you using ripgrep on?

```
$ sw_vers
ProductName:	Mac OS X
ProductVersion:	10.15.3
```

#### Describe your question, feature request, or bug.

I currently have a script pipeline that outperforms both vanilla `rg` and `git grep` while combining the latter's support for grouped boolean operators.

It would be useful if RipGrep supported these operators natively, so that composition of expressions could be more powerful.  Consider:

```
rg -e malloc --and --not '(' -e sizeof -e '\b1\b' ')'
```

The functionality supported by `git grep` is documented in the man pages, but the relevant excerpt has been reproduced below:

```
                  [--and|--or|--not|(|)|-e <pattern>...]
...
       -e
           The next parameter is the pattern. This option has to be used for
           patterns starting with - and should be used in scripts passing user
           input to grep. Multiple patterns are combined by or.

       --and, --or, --not, ( ... )
           Specify how multiple patterns are combined using Boolean
           expressions.  --or is the default operator.  --and has higher
           precedence than --or.  -e has to be used for all patterns.
```

Edit: Missing quotes

---

_Label `duplicate` added by @BurntSushi on 2020-01-28 00:17_

---

_Comment by @BurntSushi on 2020-01-28 00:17_

Duplicate of #875 

---

_Closed by @BurntSushi on 2020-01-28 00:17_

---

_Comment by @zachriggle on 2020-01-28 00:24_

Whoops, I should have searched first.  Thanks for the quick response and the awesome tool!

---
