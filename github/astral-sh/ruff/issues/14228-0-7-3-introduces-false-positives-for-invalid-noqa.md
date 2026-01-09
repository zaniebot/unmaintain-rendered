---
number: 14228
title: "`0.7.3` introduces false positives for invalid `# noqa` directives with `RUF100`"
type: issue
state: closed
author: BryceBeagle
labels:
  - bug
assignees: []
created_at: 2024-11-09T17:01:28Z
updated_at: 2024-11-09T17:52:25Z
url: https://github.com/astral-sh/ruff/issues/14228
synced_at: 2026-01-07T13:12:16-06:00
---

# `0.7.3` introduces false positives for invalid `# noqa` directives with `RUF100`

---

_Issue opened by @BryceBeagle on 2024-11-09 17:01_

I'm working on upgrading our codebase to use `ruff=0.7.3`, but there are some `RUF100` changes that start flagging
some comments as being "Unused `noqa` directive"s.

I'm not sure if this is intentional (perhaps there's a sanctioned way of adding a comment we're not using?), but this feels like a regression to me. 

This doesn't happen in `0.7.2`.
I'm assuming this is related to https://github.com/astral-sh/ruff/pull/12809

Examples:
```patch
--- blah.py
+++ blah.py
@@ -118,7 +118,7 @@
                     if res is not None:
                         res.release()

-                if 200 <= res.status < 300:  # noqa: PLR2004 Use raise_for_status
+                if 200 <= res.status < 300:  # noqa: PLR2004 raise_for_status
                     return
                 if (
                     res.status not in self._retry_status_codes

Would fix 1 error.
```

```patch
--- foobar.py
+++ foobar.py
@@ -43,7 +43,7 @@

     @inject
     def download(self) -> Any:
-        temp_file = tempfile.NamedTemporaryFile(suffix=".tar.gz", delete=False)  # noqa: SIM115 Closing right below
+        temp_file = tempfile.NamedTemporaryFile(suffix=".tar.gz", delete=False)  # noqa: SIM115 right below
         temp_file.close()

         key = f"{self.base_commit}.tar.gz"

Would fix 1 error.
```
```patch
--- fake_name.py
+++ fake_name.py
@@ -47,7 +47,7 @@
         applied_manifests
     )

-    manifest_yaml_file = tempfile.NamedTemporaryFile(  # noqa: SIM115 We're closing the file below
+    manifest_yaml_file = tempfile.NamedTemporaryFile(  # noqa: SIM115're closing the file below
         prefix="manifest_yaml_", delete=False
     )
     manifest_yaml_file.close()
```
```
blah.py:121:46: RUF100 [*] Unused `noqa` directive (unknown: `Use`)
foobar.py:46:82: RUF100 [*] Unused `noqa` directive (unknown: `Closing`)
fake_name:50:56: RUF100 [*] Unused `noqa` directive (unknown: `We`)     
```

---

_Comment by @charliermarsh on 2024-11-09 17:12_

Yeah definitely, sorry about that.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 17:12_

---

_Label `bug` added by @charliermarsh on 2024-11-09 17:12_

---

_Comment by @charliermarsh on 2024-11-09 17:26_

(You can probably work around it for now by adding a `#` in between, like: `# noqa: SIM115  # We're closing the file below` or `# noqa: SIM115 - We're closing the file below`).

---

_Referenced in [astral-sh/ruff#14229](../../astral-sh/ruff/pulls/14229.md) on 2024-11-09 17:38_

---

_Comment by @BryceBeagle on 2024-11-09 17:41_

Is adding a `#` recommended _in general_? Happy to update our pragmas to align with a recommended standard, if there is one

---

_Comment by @charliermarsh on 2024-11-09 17:43_

I tend to do it because it disambiguates the content from the pragma (and it's easier for us to parse without any ambiguity). It's also nice if you have _multiple_ pragmas on the same line, like `# noqa  # type: ignore` or something. But I don't know that it's an "official" recommendation :)

---

_Closed by @charliermarsh on 2024-11-09 17:49_

---

_Comment by @BryceBeagle on 2024-11-09 17:52_

Thanks so much for the rapid fix! You're awesome 🎉 

---

_Referenced in [astral-sh/ruff#14320](../../astral-sh/ruff/issues/14320.md) on 2024-11-13 14:16_

---

_Referenced in [canonical/craft-application#557](../../canonical/craft-application/pulls/557.md) on 2024-11-15 15:35_

---
