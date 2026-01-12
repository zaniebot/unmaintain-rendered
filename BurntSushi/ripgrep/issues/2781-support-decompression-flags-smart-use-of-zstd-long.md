```yaml
number: 2781
title: Support decompression flags / smart use of zstd --long
type: issue
state: closed
author: catleeball
labels: []
assignees: []
created_at: 2024-04-17T18:19:00Z
updated_at: 2024-04-17T20:59:35Z
url: https://github.com/BurntSushi/ripgrep/issues/2781
synced_at: 2026-01-12T16:13:24Z
```

# Support decompression flags / smart use of zstd --long

---

_@catleeball_

### Issue

When uzing `rg --search-zip` to search `.zst` files, if the file was compressed with special settings like `--long`, trying to decompress will fail[1] unless you use the corresponding `--long` flag when decompressing[2].


### Feature request

It would be nice if either:

1. rg would allow me to pass the compression tool some flags, or
2. rg could somehow detect if a flag like --long was needed and apply it on decompression

Pursing 1 might also be useful for things like passing a dictionary to zstd.

I also opened [a bug](https://github.com/facebook/zstd/issues/4028) in the zstd repo to see if they can do some nicer auto-detection. It seems like there could be other useful use-cases for this feature in ripgrep though.

I know this might be niche! But I figured I would raise the issue for people like me who are trying to search corpora that have been aggressively compressed. Then I can just rg the corpora directory like usual without doing some fiddly shell scripting to decompress things properly. :)

Thank you!


### Footnotes

[1] Example error:

```
-------------------------------------------------------------------------------                                                                                                               
./submissions/RS_2018-01.zst:                                                                                                                                                                 
-------------------------------------------------------------------------------                                                                                                               
sions/RS_2018-01.zst : Decoding error (36) : Frame requires too much memory for decoding                                                                                                      
sions/RS_2018-01.zst : Window size larger than maximum : 2147483648 > 134217728                                                                                                               
sions/RS_2018-01.zst : Use --long=31 or --memory=2048MB                                                                                                                                       
-------------------------------------------------------------------------------
```

[2] In this case, like the error message suggests, I can decompress these files if I use the `--long=31` flag.

---

_Comment by @BurntSushi on 2024-04-17 20:59_

This seems like something `zstd` should be doing. And even if it can't or won't, you can use the `--pre` flag to handle this case.

---

_Closed by @BurntSushi on 2024-04-17 20:59_

---
