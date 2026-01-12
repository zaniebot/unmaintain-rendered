```yaml
number: 3175
title: Do not fail when replace was not successful
type: issue
state: closed
author: pkuczynski
labels:
  - wontfix
assignees: []
created_at: 2025-10-10T11:04:36Z
updated_at: 2025-10-10T13:17:11Z
url: https://github.com/BurntSushi/ripgrep/issues/3175
synced_at: 2026-01-12T16:13:25Z
```

# Do not fail when replace was not successful

---

_@pkuczynski_

#### Describe your feature request

Currently when `-r` flag is passed to replace a matched string, when no match is found, ripgrep fails with code `1` and it breaks pipe excution. I am using ripgrep to clean the found output, wich sometimes might contain some text, which I want to get rid of:


```bash
ansifilter some.log | rg -oN "some pattern" | rg ' \(?[0-9]+ *(ms|s)\)?$' -r '' | sort | uniq -d
```

The second `rg` call is to remove duration marks, which sometimes exist, and sometimes not. When they don't above command would fail. As a workaround I am using:

```bash
ansifilter some.log | rg -oN "some pattern" | { rg ' \(?[0-9]+ *(ms|s)\)?$' -r '' || true } | sort | uniq -d
```

This works, but I would prefer to have a switch (eg: `--ignore-missing` or `-R` as opposite to `-r`) and silently ignore when replace could not find a match:

```bash
ansifilter some.log | rg -oN "some pattern" | rg ' \(?[0-9]+ *(ms|s)\)?$' -R '' | sort | uniq -d
ansifilter some.log | rg -oN "some pattern" | rg ' \(?[0-9]+ *(ms|s)\)?$' -r '' --ignore-missing | sort | uniq -d
```


---

_Comment by @BurntSushi on 2025-10-10 11:41_

I don't think this is worth a flag, sorry. I think your `|| true` is exactly the right thing to do.

---

_Closed by @BurntSushi on 2025-10-10 11:41_

---

_Label `wontfix` added by @BurntSushi on 2025-10-10 11:42_

---

_Comment by @pkuczynski on 2025-10-10 13:02_

Unless you have to use it zillion of times and remember about it. Maybe additional flag is not worth it but `-R` to ignore when pattern does not exists sound like a good compromise...

---

_Comment by @BurntSushi on 2025-10-10 13:17_

ripgrep has over 100 flags. I can't add a new flag for every niche use case, especially when there is an easy work-around. And I'm definitely not using one of the few remaining unused short flags for something like this.

If you're having trouble remembering to use it and you happen to use it a lot, then the solution is to write a wrapper script with the behavior you want.

---
