```yaml
number: 2474
title: "ripgrep always adds a newline at the end of data (including when `--passthru` is used)"
type: issue
state: open
author: misaki-web
labels:
  - bug
assignees: []
created_at: 2023-03-22T23:44:55Z
updated_at: 2023-11-22T21:42:23Z
url: https://github.com/BurntSushi/ripgrep/issues/2474
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep always adds a newline at the end of data (including when `--passthru` is used)

---

_@misaki-web_

Using ripgrep 13.0.0 on Ubuntu 22.10, the argument `--passthru` adds a new line at the end of data (so it can't be used to get only changes explicitly requested with the `-r` argument). It seems to be a bug, the documentation doesn't refer to adding a newline, but only `Print both matching and non-matching lines`, implying that data should be the same at the end of the process (unless replacements occur).

Example of the issue:

```bash
$ echo -n abc$'\n'def | rg --passthru 'b' -r 'B' | xxd -g 1
00000000: 61 42 63 0a 64 65 66 0a                          aBc.def.
```

Expected result: no newline added at the end, like `sed` or other similar tools:

```bash
$ echo -n abc$'\n'def | sed 's/b/B/g' | xxd -g 1
00000000: 61 42 63 0a 64 65 66                             aBc.def
$ echo -n abc$'\n'def | perl -pe 's/b/B/g' | xxd -g 1
00000000: 61 42 63 0a 64 65 66                             aBc.def
$ echo -n abc$'\n'def | tr 'b' 'B' | xxd -g 1
00000000: 61 42 63 0a 64 65 66                             aBc.def
```

Here's an example with `diff`:

```bash
$ cat lorem.txt | xxd -g 1
00000000: 6c 6f 72 65 6d 0a 69 70 73 75 6d                 lorem.ipsum
$ rg --passthru -e lorem -r Lorem lorem.txt | sponge lorem.txt
$ git diff
```

![diff-rg](https://user-images.githubusercontent.com/64830812/227062658-1df7d8f5-90ca-4c96-b35e-a95361d67b6a.png)

The same process with `sed` results in a different output for `diff`:

```bash
$ git restore lorem.txt
$ sed -i 's/lorem/Lorem/g' lorem.txt
$ git diff
```

![diff-sed](https://user-images.githubusercontent.com/64830812/227062664-72330bdf-246d-445e-a1d3-eab89ce80ba8.png)


---

_Comment by @BurntSushi on 2023-03-22 23:51_

I can confirm that this exists on master.

This may be difficult to fix. I believe there have been past issues similar to this.

---

_Label `bug` added by @BurntSushi on 2023-03-22 23:51_

---

_Comment by @BurntSushi on 2023-11-22 21:42_

It's also worth pointing out this is not really specific to `--passthru` (and I've changed the issue title accordingly). Rather, it's what ripgrep does even without `--passthru` because the alternative is generally not great:

```
$ printf "A" | xxd
00000000: 41                                       A
$ printf "A" | rg . | xxd
00000000: 410a                                     A.
$ printf "A" | grep . | xxd
00000000: 410a                                     A.
```

Notice that grep behaves the same here, so the appeal to the behavior of other tools in this space can go either way.

This might wind up being a `wontfix` bug, and I'm pretty sure the existing behavior is actually intentional. With that said, I believe the way ripgrep deals with line terminators is generally sub-optimal, and I can agree that preserving them as they are in the input file can be useful. The gnarly part of this issue though is even if we could fix it, it's not clear that making `--passthru` better on this metric would be worth making ripgrep worse in the general case. That is, when searching arbitrary files and printing a match on the last line, you really really want to emit a line terminator even if the original doesn't end with one.

Either way, this is unlikely to be fixed any time soon.

---

_Renamed from ""--passthru" adds a newline at the end of data" to "ripgrep always adds a newline at the end of data (including when `--passthru` is used)" by @BurntSushi on 2023-11-22 21:42_

---
