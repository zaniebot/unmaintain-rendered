```yaml
number: 9280
title: Normalise Hex and unicode escape sequences in string
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: normalalize-hex-escapes
created_at: 2023-12-26T03:56:06Z
updated_at: 2023-12-28T01:06:59Z
url: https://github.com/astral-sh/ruff/pull/9280
synced_at: 2026-01-10T23:07:18Z
```

# Normalise Hex and unicode escape sequences in string

---

_Pull request opened by @MichaReiser on 2023-12-26 03:56_

## Summary

This PR implements Black's `hex_codes_in_unicode_sequences` preview style that normalises escape sequences:

* `\u`, `\U` in non-byte literals: To use lower-case characters (`A-F` -> `a-f`)
* `\x`: To use lower-case characters (`A-F` -> `a-f`)
* `\N` in non-byte literals: To use uppercase characters 

Using lowercase characters for hex escape sequences is consistent to Python's `repr`. 
But it has the downside that it is inconsistent with how we (and Black) format numbers in hexadecimal notations: `0xAF`. 
Using lowercase characters is the right choise in my view and I'm leaning towards changing number literals to use lower case too. 
But we need to be careful with this because it seems black changed [their preference a few times in the past](https://github.com/psf/black/pull/2916#discussion_r827593743). 

 
Part of #8678 

## Test Plan

* `cargo test`
* The similarity index numbers are unchanged.


---

_Label `formatter` added by @MichaReiser on 2023-12-26 03:56_

---

_Label `preview` added by @MichaReiser on 2023-12-26 03:56_

---

_@MichaReiser reviewed on 2023-12-26 03:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:867 on 2023-12-26 03:59_

I tried to avoid the allocation here when the string needs to be normalized, but making it work with the outer `normalize_string` was somewhat complicated (relative string indices, different iterators etc). That's why I decided to accept the allocation here, considering that we only allocate for non-normalised (unformatted) escape sequences (rare). 

There's also the case that we need to be able to recover in case the escape sequence is invalid, which means we can't perform the normalisation in place (and directly push to the outer `output` `String`)

---

_@MichaReiser reviewed on 2023-12-26 03:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:869 on 2023-12-26 03:59_

This turned out more complicated than I hoped it would. Maybe a candidate to use a Regex? But I also think it's not worth bothering much. It's relatively straightforward code and I don't expect many changes to it,

---

_Comment by @github-actions[bot] on 2023-12-26 04:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: Selection of nursery rule `E211` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E273` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E228` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E117` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E262` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E223` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E112` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E203` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E116` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E221` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E227` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E242` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E271` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E275` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E261` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E222` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E111` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E202` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E115` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E226` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E231` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E241` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E224` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E113` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E252` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E265` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E272` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E274` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E266` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E225` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E201` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E114` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E211` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E273` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E228` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E117` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E262` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E223` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E112` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E203` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E116` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E221` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E227` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E242` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E271` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E275` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E261` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E222` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E111` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E202` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E115` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E226` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E231` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E241` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E224` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E113` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E252` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E265` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E272` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E274` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E266` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E225` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E201` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E114` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E211` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E273` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E228` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E117` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E262` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E223` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E112` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E203` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E116` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E221` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E227` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E242` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E271` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E275` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E261` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E222` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E111` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E202` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E115` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E226` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E231` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E241` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E224` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E113` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E252` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E265` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E272` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E274` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E266` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E225` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E201` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E114` without the `--preview` flag is deprecated.
warning: The following rules may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: The `flake8-quotes.inline-quotes="single"` option is incompatible with the formatter's `format.quote-style="double"`. We recommend disabling `Q000` and `Q003` when using the formatter, which enforces a consistent quote style. Alternatively, set both options to either `"single"` or `"double"`.
warning: Detected debug build without --no-cache.
error: Failed to read tests/roots/test-pycode/cp_1251_coded.py: stream did not contain valid UTF-8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+101 -101 lines in 26 files in 9 projects; 1 project error; 31 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+8 -8 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/utils.py#L484'>disnake/utils.py~L484</a>
```diff
 
 
 def _get_mime_type_for_image(data: _BytesLike) -> str:
-    if data[0:8] == b"\x89\x50\x4E\x47\x0D\x0A\x1A\x0A":
+    if data[0:8] == b"\x89\x50\x4e\x47\x0d\x0a\x1a\x0a":
         return "image/png"
     elif data[0:3] == b"\xff\xd8\xff" or data[6:10] in (b"JFIF", b"Exif"):
         return "image/jpeg"
```
<a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/tests/test_utils.py#L241'>tests/test_utils.py~L241</a>
```diff
 @pytest.mark.parametrize(
     ("data", "expected_mime", "expected_ext"),
     [
-        (b"\x89\x50\x4E\x47\x0D\x0A\x1A\x0A", "image/png", ".png"),
-        (b"\xFF\xD8\xFFxxxJFIF", "image/jpeg", ".jpg"),
-        (b"\xFF\xD8\xFFxxxExif", "image/jpeg", ".jpg"),
-        (b"\xFF\xD8\xFFxxxxxxxxxxxx", "image/jpeg", ".jpg"),
+        (b"\x89\x50\x4e\x47\x0d\x0a\x1a\x0a", "image/png", ".png"),
+        (b"\xff\xd8\xffxxxJFIF", "image/jpeg", ".jpg"),
+        (b"\xff\xd8\xffxxxExif", "image/jpeg", ".jpg"),
+        (b"\xff\xd8\xffxxxxxxxxxxxx", "image/jpeg", ".jpg"),
         (b"xxxxxxJFIF", "image/jpeg", ".jpg"),
         (b"\x47\x49\x46\x38\x37\x61", "image/gif", ".gif"),
         (b"\x47\x49\x46\x38\x39\x61", "image/gif", ".gif"),
```
<a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/tests/test_utils.py#L252'>tests/test_utils.py~L252</a>
```diff
     ],
 )
 def test_mime_type_valid(data, expected_mime, expected_ext) -> None:
-    for d in (data, data + b"\xFF"):
+    for d in (data, data + b"\xff"):
         assert utils._get_mime_type_for_image(d) == expected_mime
         assert utils._get_extension_for_image(d) == expected_ext
 
-    prefixed = b"\xFF" + data
+    prefixed = b"\xff" + data
     with pytest.raises(ValueError, match=r"Unsupported image type given"):
         utils._get_mime_type_for_image(prefixed)
     assert utils._get_extension_for_image(prefixed) is None
```
<a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/tests/test_utils.py#L265'>tests/test_utils.py~L265</a>
```diff
 @pytest.mark.parametrize(
     "data",
     [
-        b"\x89\x50\x4E\x47\x0D\x0A\x1A\xFF",  # invalid png end
+        b"\x89\x50\x4e\x47\x0d\x0a\x1a\xff",  # invalid png end
         b"\x47\x49\x46\x38\x38\x61",  # invalid gif version
         b"RIFFxxxxAAAA",
         b"AAAAxxxxWEBP",
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+6 -6 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/io.py#L220'>rasa/utils/io.py~L220</a>
```diff
     """Returns regex to identify emojis."""
     return re.compile(
         "["
-        "\U0001F600-\U0001F64F"  # emoticons
-        "\U0001F300-\U0001F5FF"  # symbols & pictographs
-        "\U0001F680-\U0001F6FF"  # transport & map symbols
-        "\U0001F1E0-\U0001F1FF"  # flags (iOS)
-        "\U00002702-\U000027B0"
-        "\U000024C2-\U0001F251"
+        "\U0001f600-\U0001f64f"  # emoticons
+        "\U0001f300-\U0001f5ff"  # symbols & pictographs
+        "\U0001f680-\U0001f6ff"  # transport & map symbols
+        "\U0001f1e0-\U0001f1ff"  # flags (iOS)
+        "\U00002702-\U000027b0"
+        "\U000024c2-\U0001f251"
         "\u200d"  # zero width joiner
         "\u200c"  # zero width non-joiner
         "]+",
```

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/lib/package/stream_cursor_utils.py#L7'>samcli/lib/package/stream_cursor_utils.py~L7</a>
```diff
 
 # NOTE: ANSI escape codes.
 # NOTE: Still needs investigation on non terminal environments.
-ESC = "\u001B["
+ESC = "\u001b["
 
 # Enables ANSI escape codes on Windows
 if platform.system().lower() == "windows":
```

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+33 -33 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/selfdrive/car/honda/carcontroller.py#L160'>selfdrive/car/honda/carcontroller.py~L160</a>
```diff
         # tester present - w/ no response (keeps radar disabled)
         if self.CP.carFingerprint in (HONDA_BOSCH - HONDA_BOSCH_RADARLESS) and self.CP.openpilotLongitudinalControl:
             if self.frame % 10 == 0:
-                can_sends.append((0x18DAB0F1, 0, b"\x02\x3E\x80\x00\x00\x00\x00\x00", 1))
+                can_sends.append((0x18DAB0F1, 0, b"\x02\x3e\x80\x00\x00\x00\x00\x00", 1))
 
         # Send steering command.
         can_sends.append(hondacan.create_steering_control(self.packer, apply_steer, CC.latActive, self.CP.carFingerprint, CS.CP.openpilotLongitudinalControl))
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/selfdrive/car/hyundai/carcontroller.py#L95'>selfdrive/car/hyundai/carcontroller.py~L95</a>
```diff
             addr, bus = 0x7D0, 0
             if self.CP.flags & HyundaiFlags.CANFD_HDA2.value:
                 addr, bus = 0x730, self.CAN.ECAN
-            can_sends.append([addr, 0, b"\x02\x3E\x80\x00\x00\x00\x00\x00", bus])
+            can_sends.append([addr, 0, b"\x02\x3e\x80\x00\x00\x00\x00\x00", bus])
 
             # for blinkers
             if self.CP.flags & HyundaiFlags.ENABLE_BLINKERS:
-                can_sends.append([0x7B1, 0, b"\x02\x3E\x80\x00\x00\x00\x00\x00", self.CAN.ECAN])
+                can_sends.append([0x7B1, 0, b"\x02\x3e\x80\x00\x00\x00\x00\x00", self.CAN.ECAN])
 
         # CAN-FD platforms
         if self.CP.carFingerprint in CANFD_CAR:
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/selfdrive/car/subaru/carcontroller.py#L168'>selfdrive/car/subaru/carcontroller.py~L168</a>
```diff
             if self.CP.flags & SubaruFlags.DISABLE_EYESIGHT:
                 # Tester present (keeps eyesight disabled)
                 if self.frame % 100 == 0:
-                    can_sends.append([GLOBAL_ES_ADDR, 0, b"\x02\x3E\x80\x00\x00\x00\x00\x00", CanBus.camera])
+                    can_sends.append([GLOBAL_ES_ADDR, 0, b"\x02\x3e\x80\x00\x00\x00\x00\x00", CanBus.camera])
 
                 # Create all of the other eyesight messages to keep the rest of the car happy when eyesight is disabled
                 if self.frame % 5 == 0:
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/selfdrive/car/toyota/carcontroller.py#L205'>selfdrive/car/toyota/carcontroller.py~L205</a>
```diff
 
         # keep radar disabled
         if self.frame % 20 == 0 and self.CP.flags & ToyotaFlags.DISABLE_RADAR.value:
-            can_sends.append([0x750, 0, b"\x0F\x02\x3E\x00\x00\x00\x00\x00", 0])
+            can_sends.append([0x750, 0, b"\x0f\x02\x3e\x00\x00\x00\x00\x00", 0])
 
         new_actuators = actuators.copy()
         new_actuators.steer = apply_steer / self.params.STEER_MAX
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/system/sensord/pigeond.py#L65'>system/sensord/pigeond.py~L65</a>
```diff
     # split up messages
     msgs = []
     while len(dat) > 0:
-        assert dat[:2] == b"\xB5\x62"
+        assert dat[:2] == b"\xb5\x62"
         msg_len = 6 + (dat[5] << 8 | dat[4]) + 2
         msgs.append(dat[:msg_len])
         dat = dat[msg_len:]
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/system/sensord/pigeond.py#L136'>system/sensord/pigeond.py~L136</a>
```diff
             self.send_with_ack(b"\xb5\x62\x06\x09\x0d\x00\x1f\x1f\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x17\x71\xd7")
 
             # clear flash memory (almanac backup)
-            self.send_with_ack(b"\xB5\x62\x09\x14\x04\x00\x01\x00\x00\x00\x22\xf0")
+            self.send_with_ack(b"\xb5\x62\x09\x14\x04\x00\x01\x00\x00\x00\x22\xf0")
 
             # try restoring backup to verify it got deleted
-            self.send(b"\xB5\x62\x09\x14\x00\x00\x1D\x60")
+            self.send(b"\xb5\x62\x09\x14\x00\x00\x1d\x60")
             # 1: failed to restore, 2: could restore, 3: no backup
             status = self.wait_for_backup_restore_status()
             if status == 1 or status == 3:
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/system/sensord/pigeond.py#L152'>system/sensord/pigeond.py~L152</a>
```diff
     pigeon.set_baud(9600)
 
     # $PUBX,41,1,0007,0003,460800,0*15\r\n
-    pigeon.send(b"\x24\x50\x55\x42\x58\x2C\x34\x31\x2C\x31\x2C\x30\x30\x30\x37\x2C\x30\x30\x30\x33\x2C\x34\x36\x30\x38\x30\x30\x2C\x30\x2A\x31\x35\x0D\x0A")
+    pigeon.send(b"\x24\x50\x55\x42\x58\x2c\x34\x31\x2c\x31\x2c\x30\x30\x30\x37\x2c\x30\x30\x30\x33\x2c\x34\x36\x30\x38\x30\x30\x2c\x30\x2a\x31\x35\x0d\x0a")
     time.sleep(0.1)
     pigeon.set_baud(460800)
 
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/system/sensord/pigeond.py#L162'>system/sensord/pigeond.py~L162</a>
```diff
     for _ in range(10):
         try:
             # setup port config
-            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x14\x00\x03\xFF\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x01\x00\x01\x00\x00\x00\x00\x00\x1E\x7F")
-            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x14\x00\x00\xFF\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x19\x35")
-            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x14\x00\x01\x00\x00\x00\xC0\x08\x00\x00\x00\x08\x07\x00\x01\x00\x01\x00\x00\x00\x00\x00\xF4\x80")
-            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x14\x00\x04\xFF\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x1D\x85")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x14\x00\x03\xff\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x01\x00\x01\x00\x00\x00\x00\x00\x1e\x7f")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x14\x00\x00\xff\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x19\x35")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x14\x00\x01\x00\x00\x00\xc0\x08\x00\x00\x00\x08\x07\x00\x01\x00\x01\x00\x00\x00\x00\x00\xf4\x80")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x14\x00\x04\xff\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x1d\x85")
             pigeon.send_with_ack(b"\xb5\x62\x06\x00\x00\x00\x06\x18")
             pigeon.send_with_ack(b"\xb5\x62\x06\x00\x01\x00\x01\x08\x22")
-            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x01\x00\x03\x0A\x24")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x00\x01\x00\x03\x0a\x24")
 
             # UBX-CFG-RATE (0x06 0x08)
-            pigeon.send_with_ack(b"\xB5\x62\x06\x08\x06\x00\x64\x00\x01\x00\x00\x00\x79\x10")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x08\x06\x00\x64\x00\x01\x00\x00\x00\x79\x10")
 
             # UBX-CFG-NAV5 (0x06 0x24)
             pigeon.send_with_ack(
-                b"\xB5\x62\x06\x24\x24\x00\x05\x00\x04\x03\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x5A\x63"
+                b"\xb5\x62\x06\x24\x24\x00\x05\x00\x04\x03\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x5a\x63"
             )
 
             # UBX-CFG-ODO (0x06 0x1E)
-            pigeon.send_with_ack(b"\xB5\x62\x06\x1E\x14\x00\x00\x00\x00\x00\x01\x03\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x3C\x37")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x39\x08\x00\xFF\xAD\x62\xAD\x1E\x63\x00\x00\x83\x0C")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x1e\x14\x00\x00\x00\x00\x00\x01\x03\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x3c\x37")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x39\x08\x00\xff\xad\x62\xad\x1e\x63\x00\x00\x83\x0c")
             pigeon.send_with_ack(
-                b"\xB5\x62\x06\x23\x28\x00\x00\x00\x00\x04\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x56\x24"
+                b"\xb5\x62\x06\x23\x28\x00\x00\x00\x00\x04\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x56\x24"
             )
 
             # UBX-CFG-NAV5 (0x06 0x24)
-            pigeon.send_with_ack(b"\xB5\x62\x06\x24\x00\x00\x2A\x84")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x23\x00\x00\x29\x81")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x1E\x00\x00\x24\x72")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x39\x00\x00\x3F\xC3")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x24\x00\x00\x2a\x84")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x23\x00\x00\x29\x81")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x1e\x00\x00\x24\x72")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x39\x00\x00\x3f\xc3")
 
             # UBX-CFG-MSG (set message rate)
-            pigeon.send_with_ack(b"\xB5\x62\x06\x01\x03\x00\x01\x07\x01\x13\x51")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x01\x03\x00\x02\x15\x01\x22\x70")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x01\x03\x00\x02\x13\x01\x20\x6C")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x01\x03\x00\x0A\x09\x01\x1E\x70")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x01\x03\x00\x0A\x0B\x01\x20\x74")
-            pigeon.send_with_ack(b"\xB5\x62\x06\x01\x03\x00\x01\x35\x01\x41\xAD")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x01\x03\x00\x01\x07\x01\x13\x51")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x01\x03\x00\x02\x15\x01\x22\x70")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x01\x03\x00\x02\x13\x01\x20\x6c")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x01\x03\x00\x0a\x09\x01\x1e\x70")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x01\x03\x00\x0a\x0b\x01\x20\x74")
+            pigeon.send_with_ack(b"\xb5\x62\x06\x01\x03\x00\x01\x35\x01\x41\xad")
             cloudlog.debug("pigeon configured")
 
             # try restoring almanac backup
-            pigeon.send(b"\xB5\x62\x09\x14\x00\x00\x1D\x60")
+            pigeon.send(b"\xb5\x62\x09\x14\x00\x00\x1d\x60")
             restore_status = pigeon.wait_for_backup_restore_status()
             if restore_status == 2:
                 cloudlog.warning("almanac backup restored")
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/system/sensord/pigeond.py#L217'>system/sensord/pigeond.py~L217</a>
```diff
 
                 # UBX-MGA-INI-TIME_UTC
                 msg = add_ubx_checksum(
-                    b"\xB5\x62\x13\x40\x18\x00"
+                    b"\xb5\x62\x13\x40\x18\x00"
                     + struct.pack(
                         "<BBBBHBBBBBxIHxxI", 0x10, 0x00, 0x00, 0x80, t_now.year, t_now.month, t_now.day, t_now.hour, t_now.minute, t_now.second, 0, 30, 0
                     )
```
<a href='https://github.com/commaai/openpilot/blob/6810c5b644fe898d87b5bddce67f8ab7dfc100c1/system/sensord/pigeond.py#L249'>system/sensord/pigeond.py~L249</a>
```diff
 
     if pigeon is not None:
         # controlled GNSS stop
-        pigeon.send(b"\xB5\x62\x06\x04\x04\x00\x00\x00\x08\x00\x16\x74")
+        pigeon.send(b"\xb5\x62\x06\x04\x04\x00\x00\x00\x08\x00\x16\x74")
 
         # store almanac in flash
-        pigeon.send(b"\xB5\x62\x09\x14\x04\x00\x00\x00\x00\x00\x21\xEC")
+        pigeon.send(b"\xb5\x62\x09\x14\x04\x00\x00\x00\x00\x00\x21\xec")
         try:
             if pigeon.wait_for_ack(ack=UBLOX_SOS_ACK, nack=UBLOX_SOS_NACK):
                 cloudlog.warning("Done storing almanac")
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+37 -37 lines across 6 files)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool.py#L1042'>Packs/CommonScripts/Scripts/BMCTool/BMCTool.py~L1042</a>
```diff
         ))
     )
     COLOR_BLACK = b"\x00"
-    COLOR_WHITE = b"\xFF"
+    COLOR_WHITE = b"\xff"
 
     def __init__(self, verbose=False, count=0, old=False, big=False, width=64):
         self.bdat = b""
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool.py#L1200'>Packs/CommonScripts/Scripts/BMCTool/BMCTool.py~L1200</a>
```diff
         d_buf = b""
         while len(data) > 0:
             if self.btype == self.BIN_CONTAINER:
-                d_buf += data[:3] + b"\xFF"
+                d_buf += data[:3] + b"\xff"
                 if len(d_buf) == 256:
                     d_out = d_buf + d_out
                     d_buf = b""
             else:
-                d_out += data[:3] + b"\xFF"
+                d_out += data[:3] + b"\xff"
             data = data[4:]
         return d_out
 
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool.py#L1214'>Packs/CommonScripts/Scripts/BMCTool/BMCTool.py~L1214</a>
```diff
         d_buf = b""
         while len(data) > 0:
             if self.btype == self.BIN_CONTAINER:
-                d_buf += data[:3] + b"\xFF"
+                d_buf += data[:3] + b"\xff"
                 if len(d_buf) == 256:
                     d_out = d_buf + d_out
                     d_buf = b""
             else:
-                d_out += data[:3] + b"\xFF"
+                d_out += data[:3] + b"\xff"
             data = data[3:]
         return d_out
 
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool.py#L1385'>Packs/CommonScripts/Scripts/BMCTool/BMCTool.py~L1385</a>
```diff
 
         self.b_log("info", False, f"Successfully exported {len(self.bmps)} files.")
         if self.big:
-            pad: bytes = b"\xFF"
+            pad: bytes = b"\xff"
             if not self.pal:
                 pad *= 4
             for i in range(len(self.bmps)):
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool.py#L1427'>Packs/CommonScripts/Scripts/BMCTool/BMCTool.py~L1427</a>
```diff
             return (
                 b"BM"
                 + pack("<L", len(data) + 122)
-                + b"\x00\x00\x00\x00\x7A\x00\x00\x00\x6C\x00\x00\x00"
+                + b"\x00\x00\x00\x00\x7a\x00\x00\x00\x6c\x00\x00\x00"
                 + pack("<L", width)
                 + pack("<L", height)
                 + b"\x01\x00\x20\x00\x03\x00\x00\x00"
                 + pack("<L", len(data))
-                + b"\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00\x00\x00\x00\x00\xFF niW"
+                + b"\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\xff\x00\x00\xff\x00\x00\xff\x00\x00\x00\x00\x00\x00\xff niW"
                 + (  # noqa: E501
                     b"\x00" * 36
                 )
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py#L1108'>Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py~L1108</a>
```diff
 @pytest.mark.parametrize(
     "data, expected_output",
     [
-        (b"\xFF\xE0\x07\x1F\xF8\x00", b"\xff\x1c\xe7\xff9\xe3\x18\xff\xc6\x1c\x00\xff"),
+        (b"\xff\xe0\x07\x1f\xf8\x00", b"\xff\x1c\xe7\xff9\xe3\x18\xff\xc6\x1c\x00\xff"),
         (b"\x00\x00\x00\x00", b"\x00\x00\x00\xff\x00\x00\x00\xff"),
     ],
 )
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py#L1131'>Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py~L1131</a>
```diff
 @pytest.mark.parametrize(
     "data, expected_output, btype",
     [
-        (b"\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00", b"", BMCContainer.BIN_CONTAINER),
+        (b"\xff\x00\x00\xff\x00\x00\xff\x00\x00\xff\x00\x00", b"", BMCContainer.BIN_CONTAINER),
         (
-            b"\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00",
+            b"\xff\x00\x00\xff\x00\x00\xff\x00\x00\xff\x00\x00",
             b"\xff\x00\x00\xff\x00\x00\xff\xff\x00\xff\x00\xff",
             BMCContainer.BMC_CONTAINER,
         ),
-        (b"\xFF\x00\x00", b"", BMCContainer.BIN_CONTAINER),
-        (b"\xFF\x00\x00", b"\xFF\x00\x00\xFF", BMCContainer.BMC_CONTAINER),
+        (b"\xff\x00\x00", b"", BMCContainer.BIN_CONTAINER),
+        (b"\xff\x00\x00", b"\xff\x00\x00\xff", BMCContainer.BMC_CONTAINER),
     ],
 )
 def test_b_parse_rgb32b(data, expected_output, btype):
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py#L1155'>Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py~L1155</a>
```diff
 @pytest.mark.parametrize(
     "data, expected_output, btype",
     [
-        (b"\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00", b"", BMCContainer.BIN_CONTAINER),
+        (b"\xff\x00\x00\xff\x00\x00\xff\x00\x00\xff\x00\x00", b"", BMCContainer.BIN_CONTAINER),
         (
-            b"\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00",
+            b"\xff\x00\x00\xff\x00\x00\xff\x00\x00\xff\x00\x00",
             b"\xff\x00\x00\xff\xff\x00\x00\xff\xff\x00\x00\xff\xff\x00\x00\xff",
             BMCContainer.BMC_CONTAINER,
         ),
-        (b"\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00", b"", BMCContainer.BIN_CONTAINER),
+        (b"\xff\x00\x00\xff\x00\x00\xff\x00\x00", b"", BMCContainer.BIN_CONTAINER),
         (
-            b"\xFF\x00\x00\xFF\x00\x00\xFF\x00\x00",
+            b"\xff\x00\x00\xff\x00\x00\xff\x00\x00",
             b"\xff\x00\x00\xff\xff\x00\x00\xff\xff\x00\x00\xff",
             BMCContainer.BMC_CONTAINER,
         ),
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py#L1184'>Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py~L1184</a>
```diff
     "data, expected_output",
     [
         (b"", (-1, 1, 0)),
-        (b"\xF5", (-1, 2, 0xF5)),
-        (b"\xFD", (0xFD, 0, 1)),
-        (b"\xF9", (0xF9, 8, 1)),
-        (b"\xF0\x00\x01", (0xF0, 256, 3)),
-        (b"\xA0", (-1, 2, 0xA0)),
+        (b"\xf5", (-1, 2, 0xF5)),
+        (b"\xfd", (0xFD, 0, 1)),
+        (b"\xf9", (0xF9, 8, 1)),
+        (b"\xf0\x00\x01", (0xF0, 256, 3)),
+        (b"\xa0", (-1, 2, 0xA0)),
         (b"\x80", (-1, 1, 0x00)),
         (b"\x00", (-1, 1, 0x00)),
         (b"\x10", (0x00, 16, 1)),
         (b"\x40", (-1, 1, 0x00)),
-        (b"\xD0", (-1, 1, 0x00)),
+        (b"\xd0", (-1, 1, 0x00)),
     ],
 )
 def test_b_unrle(data, expected_output):
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py#L1208'>Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py~L1208</a>
```diff
     "data, bbp, expected_output",
     [
         (b"", 3, b""),
-        (b"\xF5", 3, b""),
-        (b"\xFD", 3, b"\xFF\xFF\xFF"),
-        (b"\xA0", 3, b""),
+        (b"\xf5", 3, b""),
+        (b"\xfd", 3, b"\xff\xff\xff"),
+        (b"\xa0", 3, b""),
         (b"\x80", 3, b""),
         (b"\x00", 3, b""),
         (b"\x40", 3, b""),
-        (b"\xD0", 3, b""),
+        (b"\xd0", 3, b""),
     ],
 )
 def test_b_uncompress(data, bbp, expected_output):
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py#L1401'>Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py~L1401</a>
```diff
 
 def test_edge_case_cmd_0xC0():
     bmc = BMCContainer()
-    data = b"\xC0" + b"\xFF" * (3 * 64)  # cmd = 0xC0, rl = 64
+    data = b"\xc0" + b"\xff" * (3 * 64)  # cmd = 0xC0, rl = 64
     bbp = 2
     assert bmc.b_uncompress(data, bbp) == b""
 
 
 def test_edge_case_cmd_0xF6():
     bmc = BMCContainer()
-    data = b"\xF6" + b"\x01" * (1 * 64)  # cmd = 0xF6, rl = 64
+    data = b"\xf6" + b"\x01" * (1 * 64)  # cmd = 0xF6, rl = 64
     bbp = 128
     assert bmc.b_uncompress(data, bbp) == b""
 
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py#L1433'>Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py~L1433</a>
```diff
 
 def test_b_process_btype_is_bin_buf_256():
     bmcc = BMCContainer()
-    data = b"\x01" * 85 + b"\xFF" * 18 + b"\x01" * 90
+    data = b"\x01" * 85 + b"\xff" * 18 + b"\x01" * 90
     bmcc.btype = b".BIN"
 
     assert bmcc.b_parse_rgb24b(data)
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py#L1479'>Packs/CommonScripts/Scripts/BMCTool/BMCTool_test.py~L1479</a>
```diff
     bmcc = BMCContainer()
     bmcc.btype = b".BMC"
     bmcc.fname = "1.bmc"
-    bmcc.bdat = b"\x03" * 0xC + b"\xff" * (64 * 64) + b"\x0B" + b"\xff" * (64 * 64)
+    bmcc.bdat = b"\x03" * 0xC + b"\xff" * (64 * 64) + b"\x0b" + b"\xff" * (64 * 64)
     assert bmcc.b_process() is False
 
 
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/CommonScripts/Scripts/FormatURL/FormatURL.py#L469'>Packs/CommonScripts/Scripts/FormatURL/FormatURL.py~L469</a>
```diff
             bool: Is the character a valid code point.
         """
         url_code_points = ("!", "$", "&", '"', "(", ")", "*", "+", ",", "-", ".", "/", ":", ";", "=", "?", "@", "_", "~")
-        unicode_code_points = {"start": "\u00A0", "end": "\U0010FFFD"}
-        surrogate_characters = {"start": "\uD800", "end": "\uDFFF"}
-        non_characters = {"start": "\uFDD0", "end": "\uFDEF"}
+        unicode_code_points = {"start": "\u00a0", "end": "\U0010fffd"}
+        surrogate_characters = {"start": "\ud800", "end": "\udfff"}
+        non_characters = {"start": "\ufdd0", "end": "\ufdef"}
 
         if surrogate_characters["start"] <= char <= surrogate_characters["end"]:
             return False
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/MailListener_-_POP3/Integrations/MailListener_POP3/MailListener_POP3_test.py#L58'>Packs/MailListener_-_POP3/Integrations/MailListener_POP3/MailListener_POP3_test.py~L58</a>
```diff
     parts = [part]
 
     body, html, attachments = parse_mail_parts(parts)
-    assert body.replace("\uFFFD", "?") == "Foo?Bar=="
+    assert body.replace("\ufffd", "?") == "Foo?Bar=="
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/PAN-OS/Integrations/Panorama/Panorama.py#L54'>Packs/PAN-OS/Integrations/Panorama/Panorama.py~L54</a>
```diff
 PRE_POST = ""
 OUTPUT_PREFIX = "PANOS."
 UNICODE_FAIL = "\U0000274c"
-UNICODE_PASS = "\U00002714\U0000FE0F"
+UNICODE_PASS = "\U00002714\U0000fe0f"
 
 DATE_FORMAT = "%Y-%m-%dT%H:%M:%SZ"  # ISO8601 format with UTC, default in XSOAR
 QUERY_DATE_FORMAT = "%Y/%m/%d %H:%M:%S"
```
<a href='https://github.com/demisto/content/blob/dde7cd21c989c44322b4e34a1c42203424bfe80c/Packs/SecurityIntelligenceServicesFeed/Integrations/SecurityIntelligenceServicesFeed/SecurityIntelligenceServicesFeed_test.py#L158'>Packs/SecurityIntelligenceServicesFeed/Integrations/SecurityIntelligenceServicesFeed/SecurityIntelligenceServicesFeed_test.py~L158</a>
```diff
     # With limit parameter.
     with open("./TestData/response_get_object.json") as f:
         expected_response = json.load(f)
-    event_stream = [{"Records": {"Payload": b"\x68\x65\x6C\x6C\x6F\x5C\x74\x31\x32\x33\x31\x32\x5C\x6E"}, "end": {}}]
+    event_stream = [{"Records": {"Payload": b"\x68\x65\x6c\x6c\x6f\x5c\x74\x31\x32\x33\x31\x32\x5c\x6e"}, "end": {}}]
     expected_response["Payload"] = event_stream
     mocker.patch("botocore.client.BaseClient._make_api_call", return_value=expected_response)
     assert isinstance(
```

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/ibis-project/ibis/blob/c7da9d1521efd5b577ef48bc46c80b291b1dc2fd/ibis/util.py#L44'>ibis/util.py~L44</a>
```diff
 
 
 # https://www.compart.com/en/unicode/U+22EE
-VERTICAL_ELLIPSIS = "\u22EE"
+VERTICAL_ELLIPSIS = "\u22ee"
 # https://www.compart.com/en/unicode/U+2026
 HORIZONTAL_ELLIPSIS = "\u2026"
 
```

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mlflow/mlflow/blob/e66aaea7830d91ba770ba3756f93e686ac626ef2/tests/transformers/test_transformers_model_export.py#L3607'>tests/transformers/test_transformers_model_export.py~L3607</a>
```diff
 def test_save_model_card_with_non_utf_characters(tmp_path, model_name):
     # non-ascii unicode characters
     test_text = (
-        "Emoji testing! \u2728 \U0001F600 \U0001F609 \U0001F606 "
-        "\U0001F970 \U0001F60E \U0001F917 \U0001F9D0"
+        "Emoji testing! \u2728 \U0001f600 \U0001f609 \U0001f606 "
+        "\U0001f970 \U0001f60e \U0001f917 \U0001f9d0"
     )
 
     card_data: ModelCard = huggingface_hub.ModelCard.load(model_name)
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+9 -9 lines across 6 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/12d69c8b5739e7070b8ca2ff86587f828c9a96c0/pandas/io/excel/_base.py#L1372'>pandas/io/excel/_base.py~L1372</a>
```diff
     b"\x09\x00\x04\x00\x07\x00\x10\x00",  # BIFF2
     b"\x09\x02\x06\x00\x00\x00\x10\x00",  # BIFF3
     b"\x09\x04\x06\x00\x00\x00\x10\x00",  # BIFF4
-    b"\xD0\xCF\x11\xE0\xA1\xB1\x1A\xE1",  # Compound File Binary
+    b"\xd0\xcf\x11\xe0\xa1\xb1\x1a\xe1",  # Compound File Binary
 )
 ZIP_SIGNATURE = b"PK\x03\x04"
 PEEK_SIZE = max(map(len, XLS_SIGNATURES + (ZIP_SIGNATURE,)))
```
<a href='https://github.com/pandas-dev/pandas/blob/12d69c8b5739e7070b8ca2ff86587f828c9a96c0/pandas/tests/io/parser/test_c_parser_only.py#L510'>pandas/tests/io/parser/test_c_parser_only.py~L510</a>
```diff
 
 def test_buffer_rd_bytes_bad_unicode(c_parser_only):
     # see gh-22748
-    t = BytesIO(b"\xB0")
+    t = BytesIO(b"\xb0")
     t = TextIOWrapper(t, encoding="ascii", errors="surrogateescape")
     msg = "'utf-8' codec can't encode character"
     with pytest.raises(UnicodeError, match=msg):
```
<a href='https://github.com/pandas-dev/pandas/blob/12d69c8b5739e7070b8ca2ff86587f828c9a96c0/pandas/tests/io/test_stata.py#L400'>pandas/tests/io/test_stata.py~L400</a>
```diff
             [(1, 2, 3, 4)],
             columns=[
                 "good",
-                "b\u00E4d",
+                "b\u00e4d",
                 "8number",
                 "astringwithmorethan32characters______",
             ],
```
<a href='https://github.com/pandas-dev/pandas/blob/12d69c8b5739e7070b8ca2ff86587f828c9a96c0/pandas/tests/io/test_stata.py#L1361'>pandas/tests/io/test_stata.py~L1361</a>
```diff
                 )
 
     def test_write_variable_label_errors(self, mixed_frame):
-        values = ["\u03A1", "\u0391", "\u039D", "\u0394", "\u0391", "\u03A3"]
+        values = ["\u03a1", "\u0391", "\u039d", "\u0394", "\u0391", "\u03a3"]
 
         variable_labels_utf8 = {
             "a": "City Rank",
```
<a href='https://github.com/pandas-dev/pandas/blob/12d69c8b5739e7070b8ca2ff86587f828c9a96c0/pandas/tests/plotting/frame/test_frame.py#L205'>pandas/tests/plotting/frame/test_frame.py~L205</a>
```diff
             columns=columns,
             index=index,
         )
-        _check_plot_works(df.plot, title="\u03A3")
+        _check_plot_works(df.plot, title="\u03a3")
 
     @pytest.mark.slow
     @pytest.mark.parametrize("layout", [None, (-1, 1)])
```
<a href='https://github.com/pandas-dev/pandas/blob/12d69c8b5739e7070b8ca2ff86587f828c9a96c0/pandas/tests/series/test_constructors.py#L1591'>pandas/tests/series/test_constructors.py~L1591</a>
```diff
         tm.assert_series_equal(result, expected)
 
     def test_constructor_name_hashable(self):
-        for n in [777, 777.0, "name", datetime(2001, 11, 11), (1,), "\u05D0"]:
+        for n in [777, 777.0, "name", datetime(2001, 11, 11), (1,), "\u05d0"]:
             for data in [[1, 2, 3], np.ones(3), {"a": 0, "b": 1}]:
                 s = Series(data, name=n)
                 assert s.name == n
```
<a href='https://github.com/pandas-dev/pandas/blob/12d69c8b5739e7070b8ca2ff86587f828c9a96c0/pandas/tests/series/test_formats.py#L115'>pandas/tests/series/test_formats.py~L115</a>
```diff
             1,
             1.2,
             "foo",
-            "\u03B1\u03B2\u03B3",
+            "\u03b1\u03b2\u03b3",
             "loooooooooooooooooooooooooooooooooooooooooooooooooooong",
             ("foo", "bar", "baz"),
             (1, 2),
             ("foo", 1, 2.3),
-            ("\u03B1", "\u03B2", "\u03B3"),
-            ("\u03B1", "bar"),
+            ("\u03b1", "\u03b2", "\u03b3"),
+            ("\u03b1", "bar"),
         ],
     )
     def test_various_names(self, name, string_series):
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -4 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/zulip/zulip/blob/6e220f4dc1e05b2a2f174e6cbe156e13008be83d/scripts/lib/zulip_tools.py#L31'>scripts/lib/zulip_tools.py~L31</a>
```diff
 ENDC = "\033[0m"
 BLACKONYELLOW = "\x1b[0;30;43m"
 WHITEONRED = "\x1b[0;37;41m"
-BOLDRED = "\x1B[1;31m"
+BOLDRED = "\x1b[1;31m"
 BOLD = "\x1b[1m"
 GRAY = "\x1b[90m"
 
```
<a href='https://github.com/zulip/zulip/blob/6e220f4dc1e05b2a2f174e6cbe156e13008be83d/zerver/tests/test_message_send.py#L922'>zerver/tests/test_message_send.py~L922</a>
```diff
             {
                 "type": "stream",
                 "to": "Verona",
-                "topic": "Test\uFFFETopic",
+                "topic": "Test\ufffeTopic",
                 "content": "Test message",
             },
         )
```
<a href='https://github.com/zulip/zulip/blob/6e220f4dc1e05b2a2f174e6cbe156e13008be83d/zerver/tests/test_subs.py#L3982'>zerver/tests/test_subs.py~L3982</a>
```diff
         # For Cn category
         post_data_cn = {
             "subscriptions": orjson.dumps([
-                {"name": "new\uFFFEstream", "description": "this is description"}
+                {"name": "new\ufffestream", "description": "this is description"}
             ]).decode(),
             "invite_only": orjson.dumps(False).decode(),
         }
```
<a href='https://github.com/zulip/zulip/blob/6e220f4dc1e05b2a2f174e6cbe156e13008be83d/zerver/tests/test_subs.py#L4009'>zerver/tests/test_subs.py~L4009</a>
```diff
         result = self.client_patch(f"/json/streams/{stream.id}", {"new_name": "test\n\rname"})
         self.assert_json_error(result, "Invalid character in stream name, at position 5!")
         # Check for Cn characters
-        result = self.client_patch(f"/json/streams/{stream.id}", {"new_name": "test\uFFFEame"})
+        result = self.client_patch(f"/json/streams/{stream.id}", {"new_name": "test\ufffeame"})
         self.assert_json_error(result, "Invalid character in stream name, at position 5!")
 
     def test_successful_subscriptions_list(self) -> None:
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: The following rules may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: The `flake8-quotes.inline-quotes="single"` option is incompatible with the formatter's `format.quote-style="double"`. We recommend disabling `Q000` and `Q003` when using the formatter, which enforces a consistent quote style. Alternatively, set both options to either `"single"` or `"double"`.
warning: Detected debug build without --no-cache.
error: Failed to read tests/roots/test-pycode/cp_1251_coded.py: stream did not contain valid UTF-8
```

</p>
</details>




---

_Converted to draft by @MichaReiser on 2023-12-26 04:06_

---

_Marked ready for review by @MichaReiser on 2023-12-27 09:32_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/string/mod.rs`:896 on 2023-12-27 13:40_

Can this happen? I'm getting syntax errors with invalid input (both ruff and cpython)

---

_@konstin approved on 2023-12-27 13:41_

---

_Merged by @MichaReiser on 2023-12-28 01:06_

---

_Closed by @MichaReiser on 2023-12-28 01:06_

---

_Branch deleted on 2023-12-28 01:06_

---
