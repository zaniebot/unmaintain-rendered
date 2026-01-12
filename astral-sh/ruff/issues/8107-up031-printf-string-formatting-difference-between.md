```yaml
number: 8107
title: "`UP031` (printf-string-formatting) difference between Ruff and pyupgrade"
type: issue
state: closed
author: tdulcet
labels:
  - fixes
assignees: []
created_at: 2023-10-21T14:53:41Z
updated_at: 2023-12-07T03:43:22Z
url: https://github.com/astral-sh/ruff/issues/8107
synced_at: 2026-01-12T15:54:47Z
```

# `UP031` (printf-string-formatting) difference between Ruff and pyupgrade

---

_@tdulcet_

I ran Ruff on medium sized project and then pyupgrade. Ruff missed one `UP031` autofix that pyupgrade fixed. Both Ruff and pyupgrade missed 96 `UP031` (a [known problem](https://docs.astral.sh/ruff/rules/printf-string-formatting/#known-problems)) and 23 `UP032` (a few of these may be caused by #8106).

Ruff command:
```
ruff --target-version py310 --select UP --line-length 320 --unsafe-fixes --fix .
```
pyupgrade command:
```
pyupgrade --py310-plus **/*.py
```
Diff:
```diff
-       path = "%s-%s-%s.pem" % (
+       path = "{}-{}-{}.pem".format(
		safe_domain_name(cn), # common name, which should be filename safe because it is IDNA-encoded, but in case of a malformed cert make sure it's ok to use as a filename
		cert.not_valid_after.date().isoformat().replace("-", ""), # expiration date
		hexlify(cert.fingerprint(hashes.SHA256())).decode("ascii")[0:8], # fingerprint prefix
		)
```
(There were some additional differences due to Ruff not supporting the [newer pyupgrade rules](https://github.com/asottile/pyupgrade/commits/main/README.md).)

Ruff version:
```
ruff 0.1.1
```

---

_Comment by @dhruvmanila on 2023-10-30 02:54_

Thanks for reporting! It's because of the comments which is a known limitation:

https://github.com/astral-sh/ruff/blob/87772c28847a7c9ed0843a32eb25dbec7a60b5f5/crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs#L493-L498

Relevant issue: https://github.com/astral-sh/ruff/issues/6336

\cc @zanieb Do we want to downgrade the applicability level as suggested (https://github.com/astral-sh/ruff/pull/6342#discussion_r1284475389) now that we support them?

---

_Label `autofix` added by @dhruvmanila on 2023-10-30 02:54_

---

_Label `needs-decision` added by @dhruvmanila on 2023-10-30 02:54_

---

_Comment by @charliermarsh on 2023-10-30 03:59_

Yeah, I'd suggest flagging the diagnostic, and using an unsafe fix personally.

---

_Label `needs-decision` removed by @dhruvmanila on 2023-11-20 20:20_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-07 03:01_

---

_Closed by @charliermarsh on 2023-12-07 03:43_

---
