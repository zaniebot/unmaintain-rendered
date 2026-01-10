```yaml
number: 11958
title: "Manual impl of `Debug` on `Token`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/token-debug
created_at: 2024-06-21T05:29:39Z
updated_at: 2024-06-22T04:34:12Z
url: https://github.com/astral-sh/ruff/pull/11958
synced_at: 2026-01-10T21:56:00Z
```

# Manual impl of `Debug` on `Token`

---

_Pull request opened by @dhruvmanila on 2024-06-21 05:29_

## Summary

I look at the token stream a lot, not specifically in the playground but in the terminal output and it's annoying to scroll a lot to find specific location. Most of the information is also redundant.

The final format we end up with is: `<kind> <range> (flags = ...)` e.g., `String 0..4 (flags = BYTE_STRING)` where the flags part is only populated if there are any flags set.

---

_Label `internal` added by @dhruvmanila on 2024-06-21 05:29_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-21 05:29_

---

_Comment by @MichaReiser on 2024-06-21 05:59_

Hmm, I think I preferred the old one. The new representation isn't very self explanatory without e.g. knowing what `0..5` means. 

* I agree that `kind` and `Token` could be collapsed. We could take inspiration from Rust where the `Debug` implementation of `Token::String` gets outputted exactly as such. So we could change it to `Token::Lpar` 
* I agree that we should omit the flags if they're empty. 
* We could show the range as `Token::Lpar@0..5` but it's not entirely clear to me how we would want to show the flags if they're non empty

How about: `Token::Lpar@0..5(flags = ...)` which would even be more dense?

---

_Comment by @dhruvmanila on 2024-06-21 09:50_

> How about: `Token::Lpar@0..5(flags = ...)` which would even be more dense?

Hmm, I'm not sure about not having any space in between so here's without space:
```
Name@0..1,
Equal@2..3,
String@4..12(flags = BYTE_STRING),
Newline@11..12,
FStringStart@12..15(flags = F_STRING | RAW_STRING_LOWERCASE),
FStringMiddle@15..19(flags = F_STRING | RAW_STRING_LOWERCASE),
Lbrace@19..20,
Name@20..21,
Plus@22..23,
Name@24..25,
Rbrace@25..26,
FStringMiddle@26..30(flags = F_STRING | RAW_STRING_LOWERCASE),
FStringEnd@30..31(flags = F_STRING | RAW_STRING_LOWERCASE),
Newline@31..32
```

And, here's with space (I like this):
```
Name @ 0..1,
Equal @ 2..3,
String @ 4..12 (flags = BYTE_STRING),
Newline @ 12..13,
FStringStart @ 13..16 (flags = F_STRING | RAW_STRING_LOWERCASE),
FStringMiddle @ 16..20 (flags = F_STRING | RAW_STRING_LOWERCASE),
Lbrace @ 20..21,
Name @ 21..22,
Plus @ 23..24,
Name @ 25..26,
Rbrace @ 26..27,
FStringMiddle @ 27..31 (flags = F_STRING | RAW_STRING_LOWERCASE),
FStringEnd @ 31..32 (flags = F_STRING | RAW_STRING_LOWERCASE),
Newline @ 32..33
```



---

_Comment by @MichaReiser on 2024-06-21 10:54_

To me, this has a bit too much spacing. It's unclear where the `@` belongs

```
Name @ 0..1,
Equal @ 2..3,
String @ 4..12 (flags = BYTE_STRING),
Newline @ 12..13,
FStringStart @ 13..16 (flags = F_STRING | RAW_STRING_LOWERCASE),
FStringMiddle @ 16..20 (flags = F_STRING | RAW_STRING_LOWERCASE),
Lbrace @ 20..21,
Name @ 21..22,
Plus @ 23..24,
Name @ 25..26,
Rbrace @ 26..27,
FStringMiddle @ 27..31 (flags = F_STRING | RAW_STRING_LOWERCASE),
FStringEnd @ 31..32 (flags = F_STRING | RAW_STRING_LOWERCASE),
Newline @ 32..33
```

I think I would prefer a combination of the two

```
Name@0..1,
Equal@2..3,
String@4..12 (flags = BYTE_STRING),
Newline@12..13,
FStringStart@13..16 (flags = F_STRING | RAW_STRING_LOWERCASE),
FStringMiddle@16..20 (flags = F_STRING | RAW_STRING_LOWERCASE),
Lbrace@20..21,
Name@21..22,
Plus@23..24,
Name@25..26,
Rbrace@26..27,
...
```

---

_Comment by @dhruvmanila on 2024-06-21 11:44_

> To me, this has a bit too much spacing. It's unclear where the `@` belongs

I guess I'm fine with not having a space around `@` but out of curiosity what are the options where `@` could belong to in your view? I just find it comparatively easier to read the entire list quickly if there's a space, which indicates that the word ends right there.

---

_Comment by @MichaReiser on 2024-06-21 11:57_

With the space, the `@` is mainly noise to me. Maybe we just remove it and go with something closer you had in your first version? `String 4..12 (flags = ByteString)`?

---

_Comment by @dhruvmanila on 2024-06-21 11:59_

> With the space, the `@` is mainly noise to me. Maybe we just remove it and go with something closer you had in your first version? `String 4..12 (flags = ByteString)`?

Yeah, I think that's good enough.

---

_@MichaReiser approved on 2024-06-21 12:01_

---

_Comment by @github-actions[bot] on 2024-06-21 14:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @dhruvmanila on 2024-06-22 04:18_

---

_Closed by @dhruvmanila on 2024-06-22 04:18_

---

_Branch deleted on 2024-06-22 04:18_

---
