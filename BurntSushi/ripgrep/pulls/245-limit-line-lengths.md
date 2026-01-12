```yaml
number: 245
title: Limit line lengths
type: pull_request
state: closed
author: forgottenswitch
labels: []
assignees: []
base: master
head: cols
created_at: 2016-11-22T04:03:10Z
updated_at: 2016-11-22T15:53:08Z
url: https://github.com/BurntSushi/ripgrep/pull/245
synced_at: 2026-01-12T18:23:12Z
```

# Limit line lengths

---

_@forgottenswitch_

Possible fix for https://github.com/BurntSushi/ripgrep/issues/129
Python to generate a test:
```
with open("long_line_test", "w") as f:
    for n in range(1, 10):
        for i in range(1, 100):
            i_z = str(i).zfill(5)
            f.write("_{}:{}_,".format(n, i_z))
        f.write("\n")
```

---

_Comment by @BurntSushi on 2016-11-22 12:00_

@forgottenswitch Thanks for the PR! I really appreciate you taking the time to try to fix #129! :-)

Unfortunately, I think there are significant problems with the patch as-is:

1. In #129, I proposed a specification for fixing the issue, but I don't see any evidence that this was considered here. I would much rather start with a simpler solution than jump to something as complex as this.
2. Doing stuff based on the tty width seems a bit too magical for my taste. (Moreover, I don't know how to do it on Windows, and the patch as-is looks like it won't compile on Windows.)
3. The printer code has gotten *significantly* more complicated and I don't understand most of the changes you've made. I *can't* merge code that I don't understand.

Before continuing, I think we should focus entirely on my first objection, (1), and avoid getting distracted with (2) or (3) until we actually decide that this is what we want.

---

_Comment by @BurntSushi on 2016-11-22 15:42_

> The proper solution would be to make sure each match has even context before and after.
(except left- and right- most).

It seems very complex and error prone to do this for multiple matches on one very long line. Before I even *consider* supporting something like that, I would absolutely demand a well designed abstraction around it. I'd suggest we iterate on it in the issue tracker, and not move to the PR phase until we have a design that we're both happy with.

I'd also suggest we wait to do this until I get more of #162 done, which I hope to include pulling the printer out of ripgrep proper and into its own crate.

---

_Comment by @BurntSushi on 2016-11-22 15:43_

I would strongly urge you not to spend more time on this PR until we hammer out a lot more details and a proper specification. This feature is simply too complicated to do otherwise.

---

_Closed by @BurntSushi on 2016-11-22 15:43_

---

_Comment by @forgottenswitch on 2016-11-22 15:48_

What this implements instead of even left- and rightmost context,
is try to print as much as possible before the match, and all that fits after.
This should be a mistake.

---
