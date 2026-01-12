```yaml
number: 2212
title: "Ripgrep doesn't read configuration"
type: issue
state: closed
author: bound-variable
labels:
  - invalid
assignees: []
created_at: 2022-05-13T22:27:52Z
updated_at: 2022-05-13T23:17:12Z
url: https://github.com/BurntSushi/ripgrep/issues/2212
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep doesn't read configuration

---

_@bound-variable_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

From Arch community repo

#### What operating system are you using ripgrep on?

Arch

#### Describe your bug.

`rg` doesn't read from my config.

#### What are the steps to reproduce the behavior?

Run a search. Matches are printed in red instead of blue, as defined in my `rc`.

The config is located at `/home/USER/.config/ripgrep/rc`.

The config's content is this single line: `--colors=match:fg:blue`

Running `echo $RIPGREP_CONFIG_PATH` produces: `/home/USER/.config/ripgrep/rc`

Running `echo 'this is a test' | rg --debug is` produces:

![image](https://user-images.githubusercontent.com/51193876/168396895-0aac0327-661e-4d41-a971-0efd5ba5ddae.png)


---

_Comment by @BurntSushi on 2022-05-13 22:49_

Please show the output with `--trace`.

Please also try `echo ... | RIPGREP_CONFIG_PATH=... rg --trace ...` and show the output, with the dots replaced with your stuff.

This is almost certainly not a ripgrep bug, but a setup problem on your end. These types of issues have been filed in the past, and they usually boil down to the environment variable not being set or being set to the wrong value. Please debug your environment.

---

_Comment by @bound-variable on 2022-05-13 23:09_

Here you are:

![11432](https://user-images.githubusercontent.com/51193876/168400006-78afdeb6-f7ed-44fe-b3ea-5382820b16c0.png)


---

_Comment by @BurntSushi on 2022-05-13 23:10_

So... Your config file is working then... It read the color setting from it and seems to have applied it. What's the problem?

---

_Comment by @BurntSushi on 2022-05-13 23:12_

You're doing `export RIPGREP_CONFIG_PATH=/home/...`, right?

---

_Comment by @bound-variable on 2022-05-13 23:16_

Good grief, you're right

---

_Closed by @bound-variable on 2022-05-13 23:16_

---

_Label `invalid` added by @BurntSushi on 2022-05-13 23:17_

---
