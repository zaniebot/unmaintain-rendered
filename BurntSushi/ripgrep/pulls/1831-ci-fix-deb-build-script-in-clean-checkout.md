```yaml
number: 1831
title: "ci: fix deb build script in clean checkout"
type: pull_request
state: merged
author: aswild
labels: []
assignees: []
merged: true
base: master
head: pr/build-deb
created_at: 2021-03-20T17:26:37Z
updated_at: 2021-03-20T17:37:51Z
url: https://github.com/BurntSushi/ripgrep/pull/1831
synced_at: 2026-01-12T18:23:14Z
```

# ci: fix deb build script in clean checkout

---

_@aswild_

If ripgrep hasn't been built yet (i.e. target/debug/ doesn't exist),
then cargo-out-dir can't find OUT_DIR and the copy commands fail. Fix by
running cargo build before finding OUT_DIR.

Also add a check to fail early with a sensible error message when
asciidoctor isn't installed, rather than failing because of a missing
rg.1 file after the build.

---

_Comment by @aswild on 2021-03-20 17:30_

Example output when running the current version of build-deb from a clean checkout:

```
% ci/build-deb                                                                                                                                                 
find: ‘target/debug/’: No such file or directory                                                                                                               
   Compiling memchr v2.3.4                                                                                                                                     
---snip---                                                                                    
    Finished dev [unoptimized + debuginfo] target(s) in 12.03s                                                                                                 
cp: cannot stat './rg.1': No such file or directory                                                                                                            
cp: cannot stat './rg.bash': No such file or directory                                                                                                         
cp: cannot stat './rg.fish': No such file or directory                                                
```

Also the asciidoctor check could be expanded to allow `a2x` as well since build.rs can fall back to that, whichever you prefer.

---

_Review comment by @BurntSushi on `ci/build-deb`:23 on 2021-03-20 17:33_

This LGTM. The `a2x` support in the build script is legacy I think, so it's likely to be removed.

---

_@BurntSushi approved on 2021-03-20 17:37_

Lovely, nice catch. Thank you!

---

_Merged by @BurntSushi on 2021-03-20 17:37_

---

_Closed by @BurntSushi on 2021-03-20 17:37_

---
