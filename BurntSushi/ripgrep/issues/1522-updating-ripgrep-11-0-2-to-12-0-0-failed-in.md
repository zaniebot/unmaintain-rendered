```yaml
number: 1522
title: Updating ripgrep 11.0.2 to 12.0.0 failed in homebrew Ubuntu
type: issue
state: closed
author: flowerinthenight
labels:
  - invalid
assignees: []
created_at: 2020-03-18T03:25:42Z
updated_at: 2020-03-19T16:39:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1522
synced_at: 2026-01-12T16:13:23Z
```

# Updating ripgrep 11.0.2 to 12.0.0 failed in homebrew Ubuntu

---

_@flowerinthenight_

#### What version of ripgrep are you using?
```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?
Homebrew (Linux)

#### What operating system are you using ripgrep on?
```
Linux local 5.3.0-7642-generic #34~1584408018~19.10~21df4b1-Ubuntu SMP Tue Mar 17 13:38:51 UTC  x86_64 x86_64 x86_64 GNU/Linux
```

#### Describe your question, feature request, or bug.
When updating ripgrep in homebrew, the update process failed due to a dependency.

#### If this is a bug, what are the steps to reproduce the behavior?
Run `brew upgrade -v ripgrep`

#### If this is a bug, what is the actual behavior?
```
$ brew upgrade -v ripgrep
==> Upgrading 1 outdated package:
ripgrep 11.0.2 -> 12.0.0
==> Upgrading ripgrep 11.0.2 -> 12.0.0 
rm /home/linuxbrew/.linuxbrew/bin/rg
rm /home/linuxbrew/.linuxbrew/etc/bash_completion.d/rg.bash
rm /home/linuxbrew/.linuxbrew/share/fish/vendor_completions.d/rg.fish
rm /home/linuxbrew/.linuxbrew/share/zsh/site-functions/_rg
==> Installing dependencies for ripgrep: asciidoc and docbook-xsl
asciidoc: macOS is required.
ln -s ../../Cellar/ripgrep/11.0.2/etc/bash_completion.d/rg.bash rg.bash
ln -s ../Cellar/ripgrep/11.0.2/bin/rg rg
ln -s ../../../Cellar/ripgrep/11.0.2/share/fish/vendor_completions.d/rg.fish rg.fish
ln -s ../../../Cellar/ripgrep/11.0.2/share/zsh/site-functions/_rg _rg
Error: ripgrep: An unsatisfied requirement failed this build.
==> Checking for dependents of upgraded formulae...
==> No dependents found!
```

After the first failure, I attempted to install asciidoc using `sudo apt install asciidoc` and tried again. This didn't fix the issue.

#### If this is a bug, what is the expected behavior?
Update process doesn't fail and updates to the latest available version.


---

_Comment by @BurntSushi on 2020-03-18 10:39_

I'm not sure why you're reporting this here. I don't maintain the brew package. (This repo has a binary only brew tap, but there are no asciidoc dependencies.)

Please consider finding the maintainer of the package and filling a big with them.

---

_Closed by @BurntSushi on 2020-03-18 10:39_

---

_Label `invalid` added by @BurntSushi on 2020-03-18 10:39_

---

_Comment by @Amndeep7 on 2020-03-19 16:39_

For future reference: https://github.com/Homebrew/linuxbrew-core/issues/19885

---
