```yaml
number: 1108
title: using -p option changes result output order
type: issue
state: closed
author: calvinjtaylor
labels: []
assignees: []
created_at: 2018-11-15T17:56:58Z
updated_at: 2018-11-15T18:39:47Z
url: https://github.com/BurntSushi/ripgrep/issues/1108
synced_at: 2026-01-12T16:13:22Z
```

# using -p option changes result output order

---

_@calvinjtaylor_

#### What version of ripgrep are you using?

ripgrep 0.10.0

#### How did you install ripgrep?

brew install ripgrep

#### What operating system are you using ripgrep on?

macos 

#### Describe your question, feature request, or bug.

bug, rg copy and rg -p copy change what is returned.  -p is supposed to be for pretty print, but the output is changed from default.

#### If this is a bug, what are the steps to reproduce the behavior?
the following list the commands used and the last few lines of output, which should be identical.
```
rg copy
...
176:	private List<String> copyList(List<String> list)
178:		List<String> copy = new ArrayList<String>(list.size());
181:			copy.add(s);
183:		return copy;

rg -p copy
...
67:					FileUtils.copyFile( file, teplatedFile, true );
73:						"Failed to copy the %s pluggablesources file to %s directory", file.getName(), teplatedFile.getAbsolutePath() );
```


The two commands should be identical except one with pretty output?


---

_Comment by @BurntSushi on 2018-11-15 18:39_

ripgrep's output order is not deterministic. Try running `rg copy` multiple times. The `-p` flag is a red herring.

If you want a stable order, then use `--sort path`.

---

_Closed by @BurntSushi on 2018-11-15 18:39_

---
