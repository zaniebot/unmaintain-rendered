```yaml
number: 3089
title: Add type list support for ColdFusion/CFML and BoxLang
type: issue
state: closed
author: JamoCA
labels: []
assignees: []
created_at: 2025-07-04T23:39:31Z
updated_at: 2025-07-04T23:45:44Z
url: https://github.com/BurntSushi/ripgrep/issues/3089
synced_at: 2026-01-12T16:13:25Z
```

# Add type list support for ColdFusion/CFML and BoxLang

---

_@JamoCA_

Please consider adding built-in "Type List" support for [ColdFusion](https://www.adobe.com/products/coldfusion-family.html)/[CFML](https://www.lucee.org/) and [BoxLang](https://boxlang.io/) file types.

```
cfml: *.cfc, *.cfm

boxlang: *.bx, *.bxm, *.bxs
```

If "case" is important, please update it to accept either character (ie, `[Cc][Ff][Cc]`) to support older code (as some CFML files could be ~30 years old).

*I considered using `--type-add`, but it's not persistent and the flag only applies to the current command.*

Thanks.

---

_Comment by @BurntSushi on 2025-07-04 23:45_

I don't keep issues tracking the default file type list. It's too much and not worth it. If you want things added to the default type list, then please submit a PR.

---

_Closed by @BurntSushi on 2025-07-04 23:45_

---
