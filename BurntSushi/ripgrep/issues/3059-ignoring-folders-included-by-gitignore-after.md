```yaml
number: 3059
title: ignoring folders included by .gitignore after global ignore
type: issue
state: closed
author: Mehuge
labels:
  - wontfix
assignees: []
created_at: 2025-05-30T09:04:03Z
updated_at: 2025-05-30T12:25:48Z
url: https://github.com/BurntSushi/ripgrep/issues/3059
synced_at: 2026-01-12T16:13:25Z
```

# ignoring folders included by .gitignore after global ignore

---

_@Mehuge_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

14.1.0 (rev e50df40a19)

### How did you install ripgrep?

chocolatey

### What operating system are you using ripgrep on?

Windows 11

### Describe your bug.

When .gitignore ignores things, then explicitly includes subthings, rg does not search the subthings.

e.g.

```.gitignore
*
!searchme
```

The searchme folder won't be searched.

### What are the steps to reproduce the behavior?

Run the following bash script

```bash
#!/bin/bash
mkdir /tmp/rgtest
cd /tmp/rgtest
echo -e "*\nsearchme" > .gitignore
git init
mkdir searchme
echo hello > searchme/hello.txt
rg hello
```

### What is the actual behavior?

Nothing is searched.

### What is the expected behavior?

searchme folder should be searched

---

_Comment by @BurntSushi on 2025-05-30 12:25_

Your repo omits the leading `!`, so in that case, `searchme` is never whitelisted.

But the problem here is that while `searchme` itself is whitelisted and ripgrep _does_ descend into it (confirmed by running with `--debug`), the `*` rule still applies to things within `searchme`. That is, whitelisting `searchme` doesn't automatically negate every previous ignore pattern for everything within `searchme`. That would be undesirable in most cases, even though it is pretty weird in this case.

The solution is to _also_ add `!searchme/**` for your specific case. That will forcefully override previous ignore patterns.

---

_Closed by @BurntSushi on 2025-05-30 12:25_

---

_Label `wontfix` added by @BurntSushi on 2025-05-30 12:25_

---

_Closed by @BurntSushi on 2025-05-30 12:25_

---
