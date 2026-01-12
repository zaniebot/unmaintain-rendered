```yaml
number: 2326
title: Option to encode/decode filenames
type: issue
state: closed
author: strboul
labels: []
assignees: []
created_at: 2022-10-09T10:40:16Z
updated_at: 2022-10-09T10:43:21Z
url: https://github.com/BurntSushi/ripgrep/issues/2326
synced_at: 2026-01-12T16:13:24Z
```

# Option to encode/decode filenames

---

_@strboul_

I have a list of files that names are base64 encoded. I want to have them decoded in the rg output.

```sh
file1="$(echo -n 'file1' | base64)"
file2="$(echo -n 'file2' | base64)"
echo 'foobar' > "$file1"
echo 'also foobar' > "$file2"
ls
# ZmlsZTE=  ZmlsZTI=
```

Current output:
```sh
rg foobar
# ZmlsZTE=
# 1:foobar
#
# ZmlsZTI=
# 1:also foobar
```

Wished output:
```sh
rg foobar ...
# file1
# 1:also foobar
#
# file2
# 1:foobar
```

I don't know if it's currently possible but I couldn't find this in the man files/issues.


---

_Comment by @BurntSushi on 2022-10-09 10:43_

This is definitely out of rg's scope and is way too niche to justify special handling inside ripgrep.

I suggest you run another tool on the output of ripgrep that decodes file names for you.

---

_Closed by @BurntSushi on 2022-10-09 10:43_

---
