```yaml
number: 6992
title: Fix test that checks for black and linter compatibility
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: black-compatibility
created_at: 2023-08-29T18:35:51Z
updated_at: 2023-09-29T14:50:10Z
url: https://github.com/astral-sh/ruff/pull/6992
synced_at: 2026-01-12T02:39:09Z
```

# Fix test that checks for black and linter compatibility

---

_Pull request opened by @zanieb on 2023-08-29 18:35_

This test was disabled because the fixture files moved; here I enable the test again and update the directory to cover our the correct Python fixtures.

I also updated the output to be usable and display all mismatches on failure instead of just the first one.

Before
```
thread 'test_ruff_black_compatibility' panicked at 'assertion failed: `(left == right)`
  left: `Ok("# Errors\nprint(\"foo %(foo)d bar %(bar)d\" % {\"foo\": \"1\", \"bar\": \"2\"})\n\n\"foo {:e} bar {}\".format(\"1\", 2)\n\n\"%d\" % \"1\"\n\"{:o}\".format(\"1\")\n\"%(key)d\" % {\"key\": \"1\"}\nf\"{1.1:x}\"\nf\"{1.1:x}\"\n\"%d\" % []\n\"%d\" % ([],)\n\"%(key)d\" % {\"key\": []}\nprint(\"%d\" % (\"{}\".format(\"nested\"),))\n\"%d\" % ((1, 2, 3),)\n\"%d\" % (1 if x > 0 else [])\n\n# False negatives\nWORD = \"abc\"\n\"%d\" % WORD\n\"%d %s\" % (WORD, WORD)\nVALUES_TO_FORMAT = (1, \"2\", 3.0)\n\"%d %d %f\" % VALUES_TO_FORMAT\n\n# OK\n\"%d %s %f\" % VALUES_TO_FORMAT\n\"{}\".format(\"1\")\n\"{} {} {}\".format(\"1\", 2, 3.5)\nprint(\"%d %d\" % (1, 1.1))\nf\"{1}\"\n\"%d\" % 1\nf\"{1:f}\"\nf\"{1}\"\nf\"{1}\"\n\"%d\" % 1\n\"%(key)d\" % {\"key\": 1}\nf\"{1:f}\"\nf\"{1:f}\"\n\"%d\" % 1.1\n\"%(key)d\" % {\"key\": 1.1}\n\"%s\" % []\nf\"{[]}\"\nf\"{None}\"\nf\"{None}\"\nprint(\"{}\".format(\"{}\".format(\"nested\")))\nprint(\"{}\".format(\"%d\" % (5,)))\n\"%d %d\" % \"1\"\n\"%d%d\" % \"1\"\n\"-%f\" % time.time()\n\"{!r}\".format(object[\"dn\"])\nrf\"\\{ord(c):03o}\"\n(\"%02X\" % int(_) for _ in o)\n\"%s;range=%d-*\" % (attr, upper + 1)\n\"%d\" % (len(foo),)\n\"({!r}, {!r}, {!r}, {!r})\".format(hostname, address, username, \"$PASSWORD\")\n\"{!r}\".format(\n    {\n        \"server_school_roles\": server_school_roles,\n        \"is_school_multiserver_domain\": is_school_multiserver_domain,\n    }\n)\n\"%d\" % (1 if x > 0 else 2)\n")`,
 right: `Ok("# Errors\nprint(\"foo %(foo)d bar %(bar)d\" % {\"foo\": \"1\", \"bar\": \"2\"})\n\n\"foo {:e} bar {}\".format(\"1\", 2)\n\n\"%d\" % \"1\"\n\"{:o}\".format(\"1\")\n\"%(key)d\" % {\"key\": \"1\"}\nf\"{1.1:x}\"\nf\"{1.1:x}\"\n\"%d\" % []\n\"%d\" % ([],)\n\"%(key)d\" % {\"key\": []}\nprint(\"%d\" % (\"{}\".format(\"nested\"),))\n\"%d\" % ((1, 2, 3),)\n\"%d\" % (1 if x > 0 else [])\n\n# False negatives\nWORD = \"abc\"\n\"%d\" % WORD\n\"%d %s\" % (WORD, WORD)\nVALUES_TO_FORMAT = (1, \"2\", 3.0)\n\"%d %d %f\" % VALUES_TO_FORMAT\n\n# OK\n\"%d %s %f\" % VALUES_TO_FORMAT\n\"{}\".format(\"1\")\n\"{} {} {}\".format(\"1\", 2, 3.5)\nprint(\"%d %d\" % (1, 1.1))\nf\"{1}\"\n\"%d\" % 1\nf\"{1:f}\"\nf\"{1}\"\nf\"{1}\"\n\"%d\" % 1\n\"%(key)d\" % {\"key\": 1}\nf\"{1:f}\"\nf\"{1:f}\"\n\"%d\" % 1.1\n\"%(key)d\" % {\"key\": 1.1}\n\"%s\" % []\nf\"{[]}\"\nf\"{None}\"\nf\"{None}\"\nprint(\"{}\".format(\"{}\".format(\"nested\")))\nprint(\"{}\".format(\"%d\" % (5,)))\n\"%d %d\" % \"1\"\n\"%d%d\" % \"1\"\n\"-%f\" % time.time()\n\"{!r}\".format(object[\"dn\"])\nrf\"\\{ord(c):03o}\"\n(\"%02X\" % int(_) for _ in o)\n\"%s;range=%d-*\" % (attr, upper + 1)\n\"%d\" % (len(foo),)\n\"({!r}, {!r}, {!r}, {!r})\".format(hostname, address, username, \"$PASSWORD\")\n\"{!r}\".format(\n    {\n        \"server_school_roles\": server_school_roles,\n        \"is_school_multiserver_domain\": is_school_multiserver_domain,\n    },\n)\n\"%d\" % (1 if x > 0 else 2)\n")`: Mismatch found for /Users/mz/eng/src/astral-sh/ruff/crates/ruff/resources/test/fixtures/pylint/bad_string_format_type.py', crates/ruff_cli/tests/black_compatibility_test.rs:136:5
```


After
```
Mismatch found for /Users/mz/eng/src/astral-sh/ruff/crates/ruff/resources/test/fixtures/pyflakes/F541.py
---------- FIRST ----------
# OK
a = "abc"
b = f"ghi{'jkl'}"

# Errors
c = "def"
d = "def" + "ghi"
e = "def" + "ghi"
f = "a" "b" "c" r"d" r"e"
g = ""

# OK
g = f"ghi{123:{45}}"

# Error
h = "xyz"

v = 23.234234

# OK
f"{v:0.2f}"
f"{f'{v:0.2f}'}"

# Errors
f"{v:{'0.2f'}}"
f"{''}"
"{test}"
"{ 40 }"
"{a {x}"
"{{x}}"
""
""
("" r"")

# To be fixed
# Error: f-string: single '}' is not allowed at line 41 column 8
# f"\{{x}}"


---------- SECOND ----------
# OK
a = "abc"
b = f"ghi{'jkl'}"

# Errors
c = "def"
d = "def" + "ghi"
e = "def" + "ghi"
f = "abc" r"de"
g = ""

# OK
g = f"ghi{123:{45}}"

# Error
h = "xyz"

v = 23.234234

# OK
f"{v:0.2f}"
f"{f'{v:0.2f}'}"

# Errors
f"{v:{'0.2f'}}"
f"{''}"
"{test}"
"{ 40 }"
"{a {x}"
"{{x}}"
""
""
("" r"")

# To be fixed
# Error: f-string: single '}' is not allowed at line 41 column 8
# f"\{{x}}"


---------- DIFF ----------
@@ -6,7 +6,7 @@
 c = "def"
 d = "def" + "ghi"
 e = "def" + "ghi"
-f = "a" "b" "c" r"d" r"e"
+f = "abc" r"de"
 g = ""
 
 # OK


Error: 30 mismatches found
```



---

_Comment by @zanieb on 2023-08-29 18:41_

There are ~15 mismatches that would need to be fixed or ignored before this can be merged.

The output could be cleaned up a bit more, I just hacked together an improvement so I could see what work needs to be done to get us back to ✅ here.

I see the general idea for this as more valuable for testing with _our_ formatter in the long-term rather than Black. It's unclear if it's worth investing more into this test harness since it is relatively specific to the Black daemon.

---

_Comment by @zanieb on 2023-09-29 14:50_

Not a priority for me right now

---

_Closed by @zanieb on 2023-09-29 14:50_

---
