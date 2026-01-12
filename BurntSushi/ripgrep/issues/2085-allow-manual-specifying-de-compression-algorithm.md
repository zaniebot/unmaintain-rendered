```yaml
number: 2085
title: "Allow manual specifying de-compression algorithm with '--search-zip=<alg>'"
type: issue
state: closed
author: sluongng
labels:
  - wontfix
assignees: []
created_at: 2021-11-24T16:48:12Z
updated_at: 2021-11-25T21:56:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2085
synced_at: 2026-01-12T16:13:24Z
```

# Allow manual specifying de-compression algorithm with '--search-zip=<alg>'

---

_@sluongng_

#### Describe your feature request

There are some applications that I managed which use gzip to roll up log files but
when the zip file was created, it's not named with an extension.

Instead, the name could be something like `./@1234567890` and therefore, cannot be searched with ripgrep `-z, --search-zip`.
Using zgrep works fine on these files.

Rename files like this is particularly tricky because (a) it's being managed by a running application and (b) the application could be 3rd party vendor software that we have little control over.

I suggest we enable the search zip feature by allowing `--search-zip COMPRESSION_ALG` where COMPRESSION_ALG could be any among the current algorithm that ripgrep supports: `gzip`, `gzip2` etc...

This will help us avoid having to rename an application managed file while still being able to search through the zip file.

---

_Comment by @BurntSushi on 2021-11-24 17:24_

It sounds like you could use the `--pre` flag instead.

---

_Comment by @sluongng on 2021-11-25 20:42_

I didn't know about --pre flag but it does seem like a very elaborate solution. The nature of SRE ops works often involve adhoc troubleshooting so I would appreciate if you would consider my suggestion to make it a friendlier/easier UX?

i.e. if I have to use `rg --pre`, i would just use `zgrep` instead for ergonomic at the price of a slower search speed.

---

_Comment by @BurntSushi on 2021-11-25 21:56_

`--pre` doesn't seem particularly elaborate to me, but this is why it exists: to customize how ripgrep reads files. The `-z` flag is a shortcut for a common such use case. I think yours is niche and would be better solved by pre.

---

_Closed by @BurntSushi on 2021-11-25 21:56_

---

_Label `wontfix` added by @BurntSushi on 2021-11-25 21:56_

---
