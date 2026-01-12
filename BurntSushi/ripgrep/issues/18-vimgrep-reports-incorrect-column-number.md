```yaml
number: 18
title: "--vimgrep reports incorrect column number"
type: issue
state: closed
author: latortuga
labels: []
assignees: []
created_at: 2016-09-23T16:02:59Z
updated_at: 2016-09-23T21:35:13Z
url: https://github.com/BurntSushi/ripgrep/issues/18
synced_at: 2026-01-12T18:23:11Z
```

# --vimgrep reports incorrect column number

---

_@latortuga_

The column reported is off-by-one. You can see this here:

https://github.com/BurntSushi/ripgrep/blob/master/tests/tests.rs#L577

The W in "Watsons" is column 16, vim starts column counting with 1.


---

_Comment by @BurntSushi on 2016-09-23 21:10_

And also, rg claims they are 1-based indexed:

```
    --column
        Show column numbers (1 based) in output. This only shows the column
        numbers for the first match on each line. Note that this doesn't try
        to account for Unicode. One byte is equal to one column.
```


---

_Closed by @BurntSushi on 2016-09-23 21:11_

---

_Comment by @BurntSushi on 2016-09-23 21:11_

All set. It should be fixed in the next release. I'm going to try to knock off a few bugs before cutting one.


---

_Comment by @latortuga on 2016-09-23 21:29_

Cool, thanks! Any chance of getting a linux build on the next release too? I noticed there isn't one for 0.1.17...


---

_Comment by @BurntSushi on 2016-09-23 21:35_

@latortuga Absolutely! It's supposed to happen automatically, but either Travis or rustup.rs hiccuped last time: https://travis-ci.org/BurntSushi/ripgrep/jobs/162231880


---
