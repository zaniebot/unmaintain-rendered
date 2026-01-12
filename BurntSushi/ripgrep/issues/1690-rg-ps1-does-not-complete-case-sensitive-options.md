```yaml
number: 1690
title: _rg.ps1 does not complete case sensitive options like -S. Only completes -s
type: issue
state: closed
author: jfishe
labels:
  - wontfix
assignees: []
created_at: 2020-09-26T20:10:22Z
updated_at: 2020-09-26T21:36:14Z
url: https://github.com/BurntSushi/ripgrep/issues/1690
synced_at: 2026-01-12T16:13:24Z
```

# _rg.ps1 does not complete case sensitive options like -S. Only completes -s

---

_@jfishe_

#### What version of ripgrep are you using?
ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
choco install ripgrep

ripgrep 12.1.1.20200727

#### What operating system are you using ripgrep on?

Windows 10 Home
WindowsBuildLabEx                                       : 19041.1.amd64fre.vb_release.191206-1406
WindowsCurrentVersion                                   : 6.3
WindowsEditionId                                        : Core
WindowsInstallationType                                 : Client
#### Describe your bug.

PowerShell Register-ArgumentCompleter is not case sensitive, so it thinks -s and -S are duplicates in _rg.ps1 and does not complete -S.

This may be problem with clap. I don't know rust so it's beyond me to write a pull request.

#### What are the steps to reproduce the behavior?

`.  _rg.ps1`

`rg -s<Ctrl-Space>` completes `rg -s`. It should produce:

```
rg -s
s   S

Search case sensitively (default).
```
Both -s and -S are present as shown by:

```powershell
tabexpansion2 -inputScript 'rg -s' -cursorColumn 5 | Select-Object -ExpandProperty CompletionMatches

CompletionText ListItemText    ResultType ToolTip
-------------- ------------    ---------- -------
-s             s            ParameterName Search case sensitively (default…
-S             S            ParameterName Smart case search.
```

A simple fix would be to change ListItemText for 'S' to 'S '. The space makes ListItemText unique, which is all that's needed.

You could add a space to all the capitalized options as follows:

```bash
cat _rg.ps1 | sed -r "s/'([A-Z])'/'\1 '/"
```

#### What is the actual behavior?

See above. 

#### What is the expected behavior?

See above.


---

_Comment by @BurntSushi on 2020-09-26 21:36_

Thanks for the great bug report. I would encourage you to file it with the clap project. Otherwise, non-zsh completions are provided on a best effort basis and I don't have the maintenance bandwidth to track and fix bugs related to them.

---

_Closed by @BurntSushi on 2020-09-26 21:36_

---

_Label `wontfix` added by @BurntSushi on 2020-09-26 21:36_

---
