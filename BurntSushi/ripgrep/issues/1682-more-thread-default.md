```yaml
number: 1682
title: more thread default?
type: issue
state: closed
author: rdp
labels:
  - wontfix
assignees: []
created_at: 2020-09-17T15:04:34Z
updated_at: 2020-10-29T18:35:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1682
synced_at: 2026-01-12T16:13:24Z
```

# more thread default?

---

_@rdp_

I wish to make ripgrep as fast as possible by default.

Here is what I appear to be getting today with default threads on a "big box":

```
$ cat /proc/cpuinfo  | grep processor | tail -n1
processor	: 39

$ time rg something * >/dev/null

real	0m2.636s
user	0m9.328s
sys	0m18.887s

$ time rg something --threads 40 * >/dev/null

real	0m1.844s
user	0m21.137s
sys	0m32.325s
```

Perhaps it would benefit by using the number of cores available as the default?  Cheers!

---

_Comment by @BurntSushi on 2020-09-17 15:07_

Picking the number of threads to launch is a pretty finicky exercise and there are lots of weird trade offs. On some machines, for example, I have noticed diminishing returns as ripgrep uses more threads up to the core count.

So I'm not inclined to change the default absent a more thorough investigation. A single data point isn't enough. Cases like this are exactly why ripgrep exposes a `--threads` flag. You can configure it to your optimal/desired setting.

---

_Closed by @BurntSushi on 2020-09-17 15:07_

---

_Label `wontfix` added by @BurntSushi on 2020-09-17 15:07_

---

_Comment by @rdp on 2020-10-29 18:35_

Bonus points if you can determine optimal # cores dynamically... (I think in this case it benefitted because it was on an NFS share so used more bandwidth with more requests or something...)

---
