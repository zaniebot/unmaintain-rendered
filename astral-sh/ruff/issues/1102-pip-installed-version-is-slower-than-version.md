```yaml
number: 1102
title: "pip-installed version is slower than version bundled with VS Code Extension [macOS]"
type: issue
state: closed
author: eddyg
labels: []
assignees: []
created_at: 2022-12-06T04:18:43Z
updated_at: 2022-12-09T19:55:35Z
url: https://github.com/astral-sh/ruff/issues/1102
synced_at: 2026-01-12T15:54:41Z
```

# pip-installed version is slower than version bundled with VS Code Extension [macOS]

---

_@eddyg_

Not sure how to explain this one. First, I'm running macOS:

```
❯ sw_vers
ProductName:    macOS
ProductVersion: 12.6.1
BuildVersion:   21G217
```

I wanted to take advantage of some newer features (since the bundled version with the very-useful VS Code Extension is several versions behind).

So in a _venv_, I ran:

```
pip install ruff
```

Then I went to check the version with:

```
ruff --version
```

and noticed _it took around a second_, which seemed odd, so I did some comparisons:

```
❯ time ~/.vscode/extensions/charliermarsh.ruff-2022.0.20-darwin-x64/bundled/libs/bin/ruff --version
ruff 0.0.150
user=0.00s system=0.00s cpu=39% total=0.015

❯ time .venv/bin/ruff --version
ruff 0.0.163
user=0.00s system=0.00s cpu=0% total=1.047
```

Similarly:

```
❯ time .venv/bin/ruff .
user=0.01s system=0.01s cpu=1% total=1.072

❯ time ~/.vscode/extensions/charliermarsh.ruff-2022.0.20-darwin-x64/bundled/libs/bin/ruff .
A new version of ruff is available: v0.0.150 -> v0.0.163
Run to update: pip3 install --upgrade ruff
user=0.05s system=0.02s cpu=166% total=0.045
```

I added:

```
    "settings": {
        "ruff.path": [".venv/bin/ruff"],
    }
```

to my `.code-workspace` file, but it made `ruff` unusable... I would type `import sys` (to cause an error) and I would see:

```
Undefined name `i` Ruff(F821)
Undefined name `im` Ruff(F821)
Undefined name `imp` Ruff(F821)
Undefined name `impo` Ruff(F821)
Undefined name `impor` Ruff(F821)
...
```
appear one character at a time in the "Problems" tab, with a 1s or so delay between each update. I quickly commented out the `ruff.path` change to my workspace settings and am back to using 0.0.150 bundled with the extension.
 
Any ideas? _(and **thank you** for a fantastic tool!)_

---

_Comment by @charliermarsh on 2022-12-06 04:25_

Interesting... Let me think on this one!

---

_Comment by @charliermarsh on 2022-12-06 04:29_

I'm going to cut a new VS Code extension though too which will hopefully help you out in the interim...

---

_Comment by @charliermarsh on 2022-12-06 04:33_

(I'll try to get that out tonight.)

---

_Comment by @eddyg on 2022-12-06 04:48_

Definitely no rush! I just thought it was curious enough to ask about.

I had planned on installing other versions (including 0.0.150) tomorrow via `pip`, and seeing if the slowness noted above was version specific, etc.

---

_Comment by @charliermarsh on 2022-12-06 04:54_

It's very strange! I did just try pip-installing in a fresh virtualenv on my machine, and don't see any slowness, so it may have something to do with a system configuration...

---

_Comment by @eddyg on 2022-12-06 14:17_

I am going to chalk this up to _"security checks or something?"_, because I can't explain it.

This morning, `time .venv/bin/ruff --version` run just as quickly as the VS Code bundled version!

For "fun", I tried upgrading/downgrading and the results were... _just weird_:

Upgrade to latest:

```
❯ pip install -U ruff
  Downloading ruff-0.0.165-py3-none-macosx_10_9_x86_64.macosx_10_9_arm64.macosx_10_9_universal2.whl (7.0 MB)

❯ time ruff --version
ruff 0.0.165
user=0.00s system=0.00s cpu=0% total=1.367
❯ time ruff --version
ruff 0.0.165
user=0.00s system=0.00s cpu=0% total=1.049
❯ time ruff --version
ruff 0.0.165
user=0.00s system=0.00s cpu=0% total=1.054
```

Downgrade a version:

```
❯ pip install -Iv ruff==0.0.164
  Downloading ruff-0.0.164-py3-none-macosx_10_9_x86_64.macosx_10_9_arm64.macosx_10_9_universal2.whl (7.0 MB)

❯ time ruff --version
ruff 0.0.164
user=0.00s system=0.00s cpu=0% total=1.254
❯ time ruff --version
ruff 0.0.164
user=0.00s system=0.00s cpu=0% total=1.086
❯ time ruff --version
ruff 0.0.164
user=0.00s system=0.00s cpu=0% total=1.076
```

Back to version I was running last night:

```
❯ pip install -Iv ruff==0.0.163
  Downloading ruff-0.0.163-py3-none-macosx_10_9_x86_64.macosx_10_9_arm64.macosx_10_9_universal2.whl (7.0 MB)

❯ time ruff --version
ruff 0.0.163
user=0.00s system=0.00s cpu=0% total=1.400
❯ time ruff --version
ruff 0.0.163
user=0.00s system=0.00s cpu=0% total=1.070
❯ time ruff --version
ruff 0.0.163
user=0.00s system=0.00s cpu=0% total=1.072
```

Down to .162:

```
❯ pip install -Iv ruff==0.0.162
  Using cached ruff-0.0.162-py3-none-macosx_10_9_x86_64.macosx_10_9_arm64.macosx_10_9_universal2.whl (7.0 MB)

❯ time ruff --version
ruff 0.0.162
user=0.00s system=0.00s cpu=0% total=1.243
❯ time ruff --version
ruff 0.0.162
user=0.00s system=0.00s cpu=0% total=1.071
```

Back to .163:

```
❯ pip install -Iv ruff==0.0.163
  Using cached ruff-0.0.163-py3-none-macosx_10_9_x86_64.macosx_10_9_arm64.macosx_10_9_universal2.whl (7.0 MB)

❯ time ruff --version
ruff 0.0.163
user=0.00s system=0.00s cpu=1% total=0.397
❯ time ruff --version
ruff 0.0.163
user=0.00s system=0.00s cpu=41% total=0.016
```

Back to latest:

```
❯ pip install -Iv ruff
  Using cached ruff-0.0.165-py3-none-macosx_10_9_x86_64.macosx_10_9_arm64.macosx_10_9_universal2.whl (7.0 MB)
Successfully installed ruff-0.0.165

❯ time ruff --version
ruff 0.0.165
user=0.00s system=0.00s cpu=1% total=0.407
❯ time ruff --version
ruff 0.0.165
user=0.00s system=0.00s cpu=35% total=0.016
```

🤷 

Closing this as _"not your problem"_. Apologies for taking up your time!

---

_Closed by @eddyg on 2022-12-06 14:17_

---

_Comment by @charliermarsh on 2022-12-06 14:22_

Very weird! No apology necessary, sorry you lost time to it.

---

_Comment by @eddyg on 2022-12-09 19:55_

FWIW, wanted to follow up on this... I continue to see this behavior each time I upgrade `ruff` (but have now confirmed it happens with _other_ binaries as well). I _think_ this may be caused by the "malware prevention capabilities" of endpoint protection software that is mandated on my corporate laptop. The release notes for the latest version of it mention "improvements in classifying, blocking and alerting on execution of malicious code (by reputation)". My _theory_ is that "fresh" binaries don't yet have enough "reputation", and somehow it makes then "slow" after they are initially installed on the system.

---
