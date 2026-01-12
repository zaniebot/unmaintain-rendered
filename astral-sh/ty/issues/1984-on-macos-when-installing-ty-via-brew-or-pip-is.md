```yaml
number: 1984
title: On MacOS when installing ty via brew or pip is installing the older version
type: issue
state: closed
author: mangiucugna
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-12-17T08:26:08Z
updated_at: 2025-12-17T08:49:20Z
url: https://github.com/astral-sh/ty/issues/1984
synced_at: 2026-01-12T15:54:26Z
```

# On MacOS when installing ty via brew or pip is installing the older version

---

_@mangiucugna_

### Summary

When installing ty via pip or brew it says it's installing 0.0.2 but in reality is installing ty 0.0.1-alpha.3

How to reproduce:
```
pip install ty
ty --version
ty 0.0.1-alpha.3 (144a26d44 2025-05-15)
```

```
brew install ty
ty --version
ty 0.0.1-alpha.3 (144a26d44 2025-05-15)
```

```
curl -LsSf https://astral.sh/ty/install.sh | sh
ty --version
ty 0.0.2 (42835578d 2025-12-16)
```

### Version

_No response_

---

_Comment by @MichaReiser on 2025-12-17 08:31_

Hmm. I'm not able to reproduce this:

```
❯ pip install ty
Collecting ty
  Downloading ty-0.0.2-py3-none-macosx_11_0_arm64.whl.metadata (6.1 kB)
Downloading ty-0.0.2-py3-none-macosx_11_0_arm64.whl (9.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 9.1/9.1 MB 137.9 MB/s  0:00:00
Installing collected packages: ty
Successfully installed ty-0.0.2

❯ which ty
/Users/micha/astral/test/1984/.venv/bin/ty

test/1984 on  main [?] via 🐍 v3.14.0 (1984)
❯ ty --version
ty 0.0.2 (42835578d 2025-12-16)
```

```
❯ brew install ty
==> Auto-updating Homebrew...
Adjust how often this is run with `$HOMEBREW_AUTO_UPDATE_SECS` or disable with
`$HOMEBREW_NO_AUTO_UPDATE=1`. Hide these hints with `$HOMEBREW_NO_ENV_HINTS=1` (see `man brew`).
==> Auto-updated Homebrew!
Updated 3 taps (sourcemeta/apps, homebrew/core and homebrew/cask).
==> New Formulae
ctre: Compile-time PCRE-compatible regular expression matcher for C++
==> New Casks
elgato-studio: Capture and manage Elgato devices for content creation
opencode-desktop: AI coding agent desktop client

You have 22 outdated formulae and 1 outdated cask installed.

==> Fetching downloads for: ty
✔︎ Bottle Manifest ty (0.0.2)                                          [Downloaded    7.4KB/  7.4KB]
✔︎ Bottle ty (0.0.2)                                                   [Downloaded    9.4MB/  9.4MB]
==> Pouring ty--0.0.2.arm64_sequoia.bottle.tar.gz
🍺  /opt/homebrew/Cellar/ty/0.0.2: 11 files, 21.3MB
==> Running `brew cleanup ty`...
Disable this behaviour by setting `HOMEBREW_NO_INSTALL_CLEANUP=1`.
Hide these hints with `HOMEBREW_NO_ENV_HINTS=1` (see `man brew`).
==> Caveats
fish completions have been installed to:
  /opt/homebrew/share/fish/vendor_completions.d

❯ which ty
/opt/homebrew/bin/ty

test/1984 on  main [?]
❯ ty --version
ty 0.0.2 (Homebrew 2025-12-16)
```

Can you try running `which ty` (assuming you're on linux or macos). Maybe you have some old installation somewhere that takes precedence?

---

_Label `needs-mre` added by @MichaReiser on 2025-12-17 08:31_

---

_Label `question` added by @MichaReiser on 2025-12-17 08:31_

---

_Comment by @mangiucugna on 2025-12-17 08:49_

ok it might be that brew or pip couldn't overwrite the older installation, after I installed it with curl I cannot reproduce either...

---

_Closed by @mangiucugna on 2025-12-17 08:49_

---
