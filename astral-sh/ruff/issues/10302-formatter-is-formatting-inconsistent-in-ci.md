```yaml
number: 10302
title: Formatter is formatting inconsistent in CI
type: issue
state: closed
author: joostlek
labels:
  - bug
assignees: []
created_at: 2024-03-08T22:56:46Z
updated_at: 2024-03-09T10:04:08Z
url: https://github.com/astral-sh/ruff/issues/10302
synced_at: 2026-01-12T15:54:50Z
```

# Formatter is formatting inconsistent in CI

---

_@joostlek_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

When upgrading to ruff 0.3.1 we encountered a special formatting issue in Ruff which was only happening in the CI.

When running ruff locally it seems to favor this variant:

```py
    with patch(
        "bleak.BleakScanner.discovered_devices_and_advertisement_data",  # Must patch before we setup
        {
            "44:44:33:11:23:45": (
                MagicMock(address="44:44:33:11:23:45"),
                MagicMock(),
            )
        },
    ):
```

But when committing this and pushing this, the CI will complain that this is not correct:

```diff
diff --git a/tests/components/bluetooth/test_passive_update_coordinator.py b/tests/components/bluetooth/test_passive_update_coordinator.py
index cbe23142..f4128c98 100644
--- a/tests/components/bluetooth/test_passive_update_coordinator.py
+++ b/tests/components/bluetooth/test_passive_update_coordinator.py
@@ -142,14 +142,16 @@ async def test_unavailable_callbacks_mark_the_coordinator_unavailable(
 ) -> None:
     """Test that the coordinator goes unavailable when the bluetooth stack no longer sees the device."""
     start_monotonic = time.monotonic()
-    with patch(
-        "bleak.BleakScanner.discovered_devices_and_advertisement_data",  # Must patch before we setup
-        {
-            "44:44:33:11:23:45": (
-                MagicMock(address="44:44:33:11:23:45"),
-                MagicMock(),
-            )
-        },
+    with (
+        patch(
+            "bleak.BleakScanner.discovered_devices_and_advertisement_data",  # Must patch before we setup
+            {
+                "44:44:33:11:23:45": (
+                    MagicMock(address="44:44:33:11:23:45"),
+                    MagicMock(),
+                )
+            },
+        )
     ):
```
[CI run](https://github.com/home-assistant/core/actions/runs/8209853253/job/22456201337?pr=112690)
I mean that's strange, they usually are doing the same. But when we listen to the CI and commit what they propose we get this:

```diff
diff --git a/tests/components/bluetooth/test_passive_update_coordinator.py b/tests/components/bluetooth/test_passive_update_coordinator.py
index f4128c98..cbe23142 100644
--- a/tests/components/bluetooth/test_passive_update_coordinator.py
+++ b/tests/components/bluetooth/test_passive_update_coordinator.py
@@ -142,16 +142,14 @@ async def test_unavailable_callbacks_mark_the_coordinator_unavailable(
 ) -> None:
     """Test that the coordinator goes unavailable when the bluetooth stack no longer sees the device."""
     start_monotonic = time.monotonic()
-    with (
-        patch(
-            "bleak.BleakScanner.discovered_devices_and_advertisement_data",  # Must patch before we setup
-            {
-                "44:44:33:11:23:45": (
-                    MagicMock(address="44:44:33:11:23:45"),
-                    MagicMock(),
-                )
-            },
-        )
+    with patch(
+        "bleak.BleakScanner.discovered_devices_and_advertisement_data",  # Must patch before we setup
+        {
+            "44:44:33:11:23:45": (
+                MagicMock(address="44:44:33:11:23:45"),
+                MagicMock(),
+            )
+        },
     ):
         await async_setup_component(hass, DOMAIN, {DOMAIN: {}})
         await hass.async_block_till_done()
```
[CI run](https://github.com/home-assistant/core/actions/runs/8209297748/job/22454549068?pr=112690)

Within the home assistant project we don't have any specific settings for the formatter (as far as I know).

Good to mention, both locally as in the CI pre-commit is used to run ruff.

Let me know if you need more information.

---

_Comment by @charliermarsh on 2024-03-08 23:19_

Thanks! There are a few open PRs from today to fix up issues in with formatting. I’m planning to review and merge them tonight (hopefully). I’ll check if they resolve this case.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-08 23:20_

---

_Label `bug` added by @charliermarsh on 2024-03-08 23:20_

---

_Comment by @MichaReiser on 2024-03-09 07:11_

Thanks for reporting, and I'm sorry for the inconvenience. I just tested the example in our playground that uses 0.3.2 and it now uses a consistent formatting. https://play.ruff.rs/99f78327-4a41-4c57-9b2c-bbfafab494a9

This is related to https://github.com/astral-sh/ruff/issues/10267


---

_Closed by @MichaReiser on 2024-03-09 07:11_

---

_Comment by @joostlek on 2024-03-09 10:04_

Ooh that also makes sense, i still run 3.11 locally and the CI just got updated to 3.12 yesterday afternoon. Thanks!

---
