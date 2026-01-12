```yaml
number: 3080
title: allow grep for cidr
type: issue
state: closed
author: diepes
labels:
  - wontfix
assignees: []
created_at: 2025-07-01T00:09:33Z
updated_at: 2025-07-01T00:11:47Z
url: https://github.com/BurntSushi/ripgrep/issues/3080
synced_at: 2026-01-12T16:13:25Z
```

# allow grep for cidr

---

_@diepes_

#### Describe your feature request

I have to search files for lines that match ip's in range e.g. 10.18.184.0/21

Would like a way to filter based on the CIDR and match all IP's that are in the range.


# Some examples of IP's for ip grep
1.2.3.4 (plain IPv4)
1.2.3.4/24 (IPv4 CIDR)
1.2.3.4m24 (IPv4 m notation)
1.2.3.4m255.255.255.0 (IPv4 m mask)
2001:db8::1 (plain IPv6)
2001:db8::1/64 (IPv6 CIDR)

I can think of three options
1.  additional flag e.g. "-n 10.0.0.0/8" 
2. some regex extension to add ip inline in regex?



---

_Comment by @BurntSushi on 2025-07-01 00:11_

It is not a goal for ripgrep to become a database of regexes. You should write the regex yourself and perhaps encode it into a wrapper script or a shell alias.

---

_Closed by @BurntSushi on 2025-07-01 00:11_

---

_Label `wontfix` added by @BurntSushi on 2025-07-01 00:11_

---
