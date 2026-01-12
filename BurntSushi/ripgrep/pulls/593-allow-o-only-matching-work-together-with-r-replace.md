```yaml
number: 593
title: Allow -o, --only-matching work together with -r, --replace
type: pull_request
state: closed
author: beyondcompute
labels: []
assignees: []
base: master
head: replace-and-only
created_at: 2017-09-01T17:37:09Z
updated_at: 2017-10-21T01:02:08Z
url: https://github.com/BurntSushi/ripgrep/pull/593
synced_at: 2026-01-12T18:23:13Z
```

# Allow -o, --only-matching work together with -r, --replace

---

_@beyondcompute_

Hello!

Thank you for your amazing work on much-needed and super-fast fully-featured regex search utility which is `ripgrep`.

However I’ve been missing a bit a feature similar to _ack’s_ helpful `--output` which allows printing only certain capture groups. That feature is discussed in https://github.com/BurntSushi/ripgrep/issues/320 and the discussion offers a solution. However it requires matching an entire line, which still might seem a bit less straightforward for some users.

Now, there is probably a reason why `-o` is made to conflict with `-r` so please excuse me if this PR is really out-of-place. However after implementing it, upon superficial gaze, things seem to be working fine:
<details>
<summary>
Examples outputs
</summary>

#### ripgrep
```bash
$ ./target/debug/rg 'of (\w+)' -Nor '$1' ./tests/hay.rs 
this
detective
luck
straw
cigar
```

#### ack
```bash
$ ack 'of (\w+)' --output '$1' ./tests/hay.rs 
this
detective
luck
straw
cigar
```
</details>

####

So I hope this has a chance of being considered. Thank you again!

---

_Comment by @BurntSushi on 2017-09-01 17:43_

I can't quite remember why we set the flags to conflict. There might be some subtle corner cases. I'd have to go back to that PR and read.

With that said, I'm not in principle against this, and I feel like this combination of flags makes sense.

---

_Comment by @ayosec on 2017-10-17 02:11_

> I can't quite remember why we set the flags to conflict.

I guess that the conflict was set because the [original implementation](https://github.com/beyondcompute/ripgrep/commit/90a11dec5e5765af560d343e879869a60363ef54) didn't add the [`if self.only_matching`](https://github.com/beyondcompute/ripgrep/blob/90a11dec5e5765af560d343e879869a60363ef54/src/printer.rs#L310-L313) condition in the [`if self.replace.is_some()`](https://github.com/beyondcompute/ripgrep/blob/90a11dec5e5765af560d343e879869a60363ef54/src/printer.rs#L291-L309) block.

The combination of `-r` and `-o` seems pretty natural for me. Actually, I tried to write `rg -o 'Enqueued (\S+)' -r '$1'` a few minutes ago, and then I saw that it was impossible :-( .


---

_Comment by @BurntSushi on 2017-10-21 01:02_

I rebased this on top of master and merged it in https://github.com/BurntSushi/ripgrep/commit/f887bc1f8673fea9969adae6e70b68e3761ba171. Thanks!

---

_Closed by @BurntSushi on 2017-10-21 01:02_

---
