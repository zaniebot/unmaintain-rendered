```yaml
number: 3186
title: "ignore: add lowercase r r_extensions"
type: pull_request
state: merged
author: luhann
labels: []
assignees: []
merged: true
base: master
head: r_extensions
created_at: 2025-10-14T18:47:53Z
updated_at: 2025-10-14T20:07:45Z
url: https://github.com/BurntSushi/ripgrep/pull/3186
synced_at: 2026-01-12T18:23:15Z
```

# ignore: add lowercase r r_extensions

---

_@luhann_

Add lowercase versions of *.Rmd and *.Rnw to the R type. 

These extensions are less common, but useful given that the *.r extension is also added as the R type.

Thanks for all the work you do on rg, if this isn't wanted I'm happy to keep using my custom r type.

---

_Comment by @BurntSushi on 2025-10-14 18:49_

Can you show me some code/repo that is using the `rmd` and `rnw` extensions?

---

_Comment by @luhann on 2025-10-14 19:04_

Sure, I use [R.nvim](https://github.com/R-nvim/R.nvim) for my development and it recognises both *.rmd and *.rnw as R files. [Knitr](https://github.com/yihui/knitr) also recognises both *.rmd and *.rnw as R files, although the actual files in the repo are .R and .Rnw/.Rmd due to tidyverse style guides. [Rmarkdown](https://github.com/rstudio/rmarkdown/blob/d01420eea96019cbb544f12eefd8a80e0c458114/NEWS.md?plain=1#L160) will also recognise .rmd as valid. 

---

_Comment by @BurntSushi on 2025-10-14 19:14_

I meant code repositories that _contain_ `rmd` and `rnw` files. But I suppose other tools recognizing those extensions is a fine approximation too.

---

_Merged by @BurntSushi on 2025-10-14 19:15_

---

_Closed by @BurntSushi on 2025-10-14 19:15_

---

_Comment by @luhann on 2025-10-14 19:21_

I don't mind continuing to take a look, but most R based repositories follow the tidyverse style guide which calls for .R, .Rnw, and .Rmd. Even finding examples for .r is challenging. 

I also did some checking for other file types that could possibly be confused for *.rmd and *.rnw, just so this change doesn't cause unexpected behaviour for anyone and I couldn't find any.

Thanks again for all the work you do.

---

_Branch deleted on 2025-10-14 19:22_

---

_Comment by @BurntSushi on 2025-10-14 20:07_

Thanks!

---
