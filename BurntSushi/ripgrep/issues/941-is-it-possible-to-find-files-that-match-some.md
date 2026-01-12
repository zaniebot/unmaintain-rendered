```yaml
number: 941
title: "Is it possible to find files that match some pattern and don't match another?"
type: issue
state: closed
author: borekb
labels:
  - duplicate
assignees: []
created_at: 2018-06-08T11:40:06Z
updated_at: 2018-06-09T23:35:23Z
url: https://github.com/BurntSushi/ripgrep/issues/941
synced_at: 2026-01-12T16:13:22Z
```

# Is it possible to find files that match some pattern and don't match another?

---

_@borekb_

This command finds all `package.json` files that declare a dependency on `chalk`:

```
rg -F '"chalk":' -g package.json --files-with-matches
```

This command finds all private packages:

```
rg -F '"private": true' -g package.json --files-without-match
```

If there a way to write a `rg` command that combines both? I.e., "list files that contain `"chalk":` but don't contain `"private": true`". Thanks.

---

_Comment by @okdana on 2018-06-08 12:14_

Not with a single call to `rg`, but you can do it by combining it with other shell commands. This will give you the intersection between the two result sets:

```sh
{
  rg -F '"chalk":' -g package.json --files-with-matches &
  rg -F '"private": true' -g package.json --files-without-match
} | awk 'seen[$0]++ == 1' # or `sort | uniq -d` if you don't mind waiting for output
```

---

_Comment by @borekb on 2018-06-08 14:24_

Thanks, I'll probably go that route. Wanted to ask first as doing it in one command would make it 2x faster.

---

_Comment by @BurntSushi on 2018-06-09 23:35_

@okdana I love that `awk` trick.

@borekb If this were to be fixed, it would be fixed by #875, if I decide to do it. Still unsure. So I'm going to close this as a duplicate.

---

_Closed by @BurntSushi on 2018-06-09 23:35_

---

_Label `duplicate` added by @BurntSushi on 2018-06-09 23:35_

---
