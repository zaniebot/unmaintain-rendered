```yaml
number: 11644
title: "Allow quotes around command-line options in `requirement.txt files`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
  - no-build
assignees: []
merged: true
base: main
head: charlie/q
created_at: 2025-02-19T22:32:13Z
updated_at: 2025-02-20T20:13:12Z
url: https://github.com/astral-sh/uv/pull/11644
synced_at: 2026-01-12T16:09:56Z
```

# Allow quotes around command-line options in `requirement.txt files`

---

_@charliermarsh_

## Summary

Closes #11592.


---

_Renamed from "Allow quotes around command-line options in requirement.txt files" to "Allow quotes around command-line options in `requirement.txt files`" by @charliermarsh on 2025-02-19 22:32_

---

_Label `compatibility` added by @charliermarsh on 2025-02-19 22:32_

---

_@nkitsaini reviewed on 2025-02-20 02:38_

---

_Review comment by @nkitsaini on `crates/uv-requirements-txt/src/lib.rs`:923 on 2025-02-20 02:38_

Escaping only works for double quotes. Single quotes preserve their content as-is. So one cannot include single quote inside single quotes.

Optional: There are a lot more weird rules to POSIX parsing, which is what pip relies on. It might be better to offload this to some shell parser. For example even in double quotes if the forward slash is followed by non-escape character, the forward slash and character both will be preserved.

Just some examples:
- `-e "foo""bar"` is valid and is equivalent to `-c foobar`
- `-e 'foo'\''bar'` is equivalent to `-c "foo'bar"`
- `-e "\ab"` preserves the forward slash also

[reference](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#:~:text=a%20token%20separator.-,2.2.2%20Single%2DQuotes,-Enclosing%20characters%20in)

---

_Label `no-build` added by @charliermarsh on 2025-02-20 05:47_

---

_@konstin approved on 2025-02-20 08:58_

---

_Merged by @charliermarsh on 2025-02-20 20:13_

---

_Closed by @charliermarsh on 2025-02-20 20:13_

---

_Branch deleted on 2025-02-20 20:13_

---
