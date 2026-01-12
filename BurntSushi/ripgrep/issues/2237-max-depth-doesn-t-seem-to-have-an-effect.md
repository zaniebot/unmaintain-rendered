```yaml
number: 2237
title: "max-depth doesn't seem to have an effect"
type: issue
state: closed
author: torqu3e
labels:
  - invalid
assignees: []
created_at: 2022-06-15T15:22:07Z
updated_at: 2022-06-15T17:20:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2237
synced_at: 2026-01-12T16:13:24Z
```

# max-depth doesn't seem to have an effect

---

_@torqu3e_

Per the man page, max-depth is the count of directories deeper than the search path. I have come across a situation where that does not seem to be the case

No results 
```
08:12:31 ✖1 $ rg -i "Offline maintenance of the DB instance is taking place" ~/dev_wip/repo-name/
08:12:43 ✖1 $ rg -i "Offline maintenance of the DB instance is taking place" ~/dev_wip/repo-name/aws/terraform/modules/
08:12:59 $ rg --max-depth 99 -i "Offline maintenance of the DB instance is taking place" ~/dev_wip/repo-name/aws/terraform/modules/
```

Going one level deeper on the search path parameter returns results
```
08:12:57 ✖1 $ rg -i "Offline maintenance of the DB instance is taking place" ~/dev_wip/repo-name/aws/terraform/modules/aws/
/Users/username/dev_wip/repo-name/aws/terraform/modules/aws/rds/instance/monitoring.tf
10:  query = "events('priority:all sources:rds Offline maintenance of the DB instance is taking place. The DB instance is currently unavailable').rollup('count').last('5m') > 0"

08:13:08 ✖1 $ rg --max-depth 99 -i "Offline maintenance of the DB instance is taking place" ~/dev_wip/repo-name/aws/terraform/modules/aws/
/Users/username/dev_wip/repo-name/aws/terraform/modules/aws/rds/instance/monitoring.tf
10:  query = "events('priority:all sources:rds Offline maintenance of the DB instance is taking place. The DB instance is currently unavailable').rollup('count').last('5m') > 0"
```

My first thought was that ripgrep only searches 9 levels deeper than root but based on above it seems to go 3 (default??) deeper than the path given and the `--max-depth` parameter does not change that.

```
$ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
```

---

_Comment by @BurntSushi on 2022-06-15 15:34_

Can you please provide a reproduction on a corpus we both can access?

Also, why did you delete the template? My suspicion is that this is somehow a `.gitignore` related bug, but since you haven't included the `--debug` output from ripgrep, it's impossible to know. I suppose an easy way to check it would be to run ripgrep with `--no-ignore`. Or even more permissively, `-uuu` (disables ignore filtering, hidden filtering and binary filtering).

---

_Comment by @torqu3e on 2022-06-15 17:14_

Apologies re the template, one did not popup when I tried creating an issue (which did seem odd to me).

It most certainly was a `.gitignore` entry of `aws/`. Completely user error/lack of coffee etc. Thank you for pointing that out, where do I send you a beverage for the troubles?

---

_Comment by @BurntSushi on 2022-06-15 17:20_

No problem. I just see a lot of gitignore related errors, so it's easier for me to sniff'em out.

As for sending beverages, I'm happy to see donations done instead. :-) See: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#donations

---

_Closed by @BurntSushi on 2022-06-15 17:20_

---

_Label `invalid` added by @BurntSushi on 2022-06-15 17:20_

---
