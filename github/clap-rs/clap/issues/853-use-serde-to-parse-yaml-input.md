---
number: 853
title: Use Serde to parse YAML input
type: issue
state: closed
author: dtolnay
labels:
  - C-enhancement
assignees: []
created_at: 2017-02-16T05:52:57Z
updated_at: 2021-12-08T20:01:33Z
url: https://github.com/clap-rs/clap/issues/853
synced_at: 2026-01-07T13:12:19-06:00
---

# Use Serde to parse YAML input

---

_Issue opened by @dtolnay on 2017-02-16 05:52_

The errors from `clap` when my YAML isn't perfect are far less helpful than they could be. This makes it difficult to iterate on a YAML input.

Using Serde to read the YAML, errors would be able to provide the line, column, and property path at which the error occurs.

### Steps to reproduce the issue

- Introduce a typo in examples/17_yaml.yml (some example below)
- cargo run --features yaml --example 17_yaml

---

###
```diff
diff --git a/examples/17_yaml.yml b/examples/17_yaml.yml
index 4e55891..7226619 100644
--- a/examples/17_yaml.yml
+++ b/examples/17_yaml.yml
@@ -28,7 +28,7 @@ args:
     - flag:
         help: demo flag argument
         short: F
-        multiple: true
+        multiple: tru
         global: true
         # Conflicts, mutual overrides, and requirements can all be defined as a
         # list, where the key is the name of the other argument
```

#### Actual clap error

> failed to convert YAML String("tru") value to a string

(this seems like a bug)

#### Serde-style error

> args[2].flag.multiple: invalid type: string "tru", expected a boolean at line 31 column 19

---

```diff
diff --git a/examples/17_yaml.yml b/examples/17_yaml.yml
index 4e55891..5b6ee43 100644
--- a/examples/17_yaml.yml
+++ b/examples/17_yaml.yml
@@ -9,7 +9,7 @@ settings:
 
 # All Args must be defined in the 'args:' list where the name of the arg, is the
 # key to a Hash object
-args:
+arguments:
     # The name of this argument, is 'opt' which will be used to access the value
     # later in your Rust code
     - opt:
```

#### Current clap error

- None, silently ignores everything I wrote

#### Serde-style error

> unknown field \`arguments\`, expected one of \`name\`, \`version\`, \`about\`, \`author\`, \`settings\`, \`args\`, \`subcommands\`, \`groups\` at line 12 column 1

---

```diff
diff --git a/examples/17_yaml.yml b/examples/17_yaml.yml
index 4e55891..a1fc412 100644
--- a/examples/17_yaml.yml
+++ b/examples/17_yaml.yml
@@ -67,7 +67,7 @@ args:
 subcommands:
     # The nae of this subcommand will be 'subcmd' which can be accessed in your
     # Rust code later
-    - subcmd:
+    subcmd:
         about: demos subcommands from yaml
         version: "0.1"
         author: Kevin K. <kbknapp@gmail.com>
```

#### Current clap error

- None, silently ignores my subcommand

#### Serde-style error

> subcommands: invalid type: map, expected a list of subcommands at line 70 column 5

---

_Comment by @kbknapp on 2017-02-16 14:40_

Thanks for the detailed write up! I'm 100% for this if it can be made in a backwards compatible way, if not 3.x shouldn't be too far on the horizon and would accept a PR to the `3x-dev` branch.

I'll mark this as "help wanted" simply because my bandwidth is limited right now with work and would be more than willing to take a look at a PR on the matter. Once I knock out a few more issues and my bandwidth opens back up, I'll take a swing at implementing this if it hasn't been done already :wink:

---

_Label `C: yaml parser` added by @kbknapp on 2017-02-16 14:40_

---

_Label `D: intermediate` added by @kbknapp on 2017-02-16 14:40_

---

_Label `P3: want to have` added by @kbknapp on 2017-02-16 14:40_

---

_Label `T: enhancement` added by @kbknapp on 2017-02-16 14:40_

---

_Added to milestone `serde` by @kbknapp on 2017-03-27 02:59_

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 18:59_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-08-22 04:17_

---

_Removed from milestone `serde` by @kbknapp on 2018-02-02 01:58_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:58_

---

_Label `W: 3.x` added by @kbknapp on 2018-02-05 15:27_

---

_Label `W: 3.x blocker` removed by @kbknapp on 2018-02-05 15:27_

---

_Comment by @kbknapp on 2018-07-22 01:14_

Blocker: https://github.com/dtolnay/serde-yaml/issues/94

---

_Removed from milestone `v3-alpha.1` by @kbknapp on 2018-07-22 01:14_

---

_Label `W: blocking on external` added by @kbknapp on 2018-07-22 01:15_

---

_Label `W: 3.x` removed by @kbknapp on 2018-07-22 01:15_

---

_Referenced in [clap-rs/clap#1630](../../clap-rs/clap/issues/1630.md) on 2020-01-10 15:02_

---

_Comment by @epage on 2021-07-20 17:26_

https://github.com/clap-rs/clap/issues/1041 would help with the lifetime blocker

---

_Referenced in [clap-rs/clap#918](../../clap-rs/clap/issues/918.md) on 2021-07-20 17:31_

---

_Referenced in [epage/clapng#70](../../epage/clapng/issues/70.md) on 2021-12-06 16:32_

---

_Comment by @epage on 2021-12-08 20:01_

We have decided to deprecate the YAML API.  For more details, see https://github.com/clap-rs/clap/issues/3087.  If there is interest in a YAML API, it will likely be broken out into a `clap_yaml` and developed independent from the core of clap.

---

_Closed by @epage on 2021-12-08 20:01_

---
