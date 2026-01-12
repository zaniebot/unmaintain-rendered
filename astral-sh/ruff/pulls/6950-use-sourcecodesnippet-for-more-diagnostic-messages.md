```yaml
number: 6950
title: "Use `SourceCodeSnippet` for more diagnostic messages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/source-code
created_at: 2023-08-28T18:54:53Z
updated_at: 2023-08-28T21:09:16Z
url: https://github.com/astral-sh/ruff/pull/6950
synced_at: 2026-01-12T02:45:38Z
```

# Use `SourceCodeSnippet` for more diagnostic messages

---

_Pull request opened by @charliermarsh on 2023-08-28 18:54_

## Summary

This ensures that we truncate the messages when they break over multiple lines, etc.

---

_Label `internal` added by @charliermarsh on 2023-08-28 18:55_

---

_Comment by @github-actions[bot] on 2023-08-28 19:12_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -3, 0 error(s))

<details><summary>content (+3, -3)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/a440187049ea5342f85cfa3a4da444509a98dbf3/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 [*] Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/a440187049ea5342f85cfa3a4da444509a98dbf3/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(ipaddress.ip_address(ip), ipaddress.IPv4Address | ipaddress.IPv6Address)`
+ <a href='https://github.com/demisto/content/blob/a440187049ea5342f85cfa3a4da444509a98dbf3/Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py#L2419'>Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py:2419:13:</a> PLR1701 [*] Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/a440187049ea5342f85cfa3a4da444509a98dbf3/Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py#L2419'>Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py:2419:13:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(e, ErrorMailboxMoveInProgress | ErrorMailboxStoreUnavailable)`
+ <a href='https://github.com/demisto/content/blob/a440187049ea5342f85cfa3a4da444509a98dbf3/Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py#L628'>Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py:628:41:</a> PLR1701 [*] Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/a440187049ea5342f85cfa3a4da444509a98dbf3/Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py#L628'>Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py:628:41:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(x, ErrorInvalidIdMalformed | ErrorItemNotFound)`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR1701 | 6 | 3 | 3 |



---

_Merged by @charliermarsh on 2023-08-28 20:22_

---

_Closed by @charliermarsh on 2023-08-28 20:22_

---

_Branch deleted on 2023-08-28 20:22_

---

_Comment by @MichaReiser on 2023-08-28 20:27_

This seems a structural problem. Should we even include code snippets in diagnostic headers?

---

_Comment by @charliermarsh on 2023-08-28 21:09_

We don't do it very often, and I agree that it's better served by emitting code frames and fixes, but it's sometimes useful!

---
