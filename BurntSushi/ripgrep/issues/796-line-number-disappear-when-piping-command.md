```yaml
number: 796
title: line number disappear when piping command
type: issue
state: closed
author: timotheecour
labels:
  - invalid
assignees: []
created_at: 2018-02-13T09:53:27Z
updated_at: 2019-11-22T00:00:58Z
url: https://github.com/BurntSushi/ripgrep/issues/796
synced_at: 2026-01-12T16:13:22Z
```

# line number disappear when piping command

---

_@timotheecour_

not sure whether it's a regression, don't remember seeing that before upgrading to ripgrep 0.8.0 

#### What version of ripgrep are you using?

ripgrep 0.8.0 (rev )
-SIMD -AVX

#### What operating system are you using ripgrep on?

osx

#### Describe your question, feature request, or bug.
rg --no-heading packages
```
source/dubregistry/web.d:563:   @auth @path("/my_packages/:packname/set_categories")
source/dubregistry/web.d:581:           redirect("/my_packages/"~_packname);
source/dubregistry/web.d:584:   @auth @path("/my_packages/:packname/set_repository") @errorDisplay!getMyPackagesPackage
source/dubregistry/web.d:595:           redirect("/my_packages/"~_packname);
source/dubregistry/registry.d:43:               // list of packages whose statistics need to be updated
source/dubregistry/registry.d:294:              // update stat distributions before score packages

```
rg --no-heading packages|cat
```
source/dubregistry/mirror.d:    enum urls = ["packages/index.json", "api/packages/search?q=foobar"];
source/dubregistry/mirror.d:            auto packs = requestHTTP(url ~ Path("api/packages/dump")).readJson().deserializeJson!(DbPackage[]);
source/dubregistry/mirror.d:            // first, remove all packages that don't exist anymore to avoid possible name conflicts
source/dubregistry/mirror.d:            // then add/update all existing packages
source/dubregistry/mirror.d:            logError("Fetching updated packages failed: %s", e.msg);
source/dubregistry/registry.d:          // list of packages whose statistics need to be updated
source/dubregistry/registry.d:          // update stat distributions before score packages
source/dubregistry/registry.d:                  "Published packages must contain \"description\" and \"license\" fields.");
source/dubregistry/registry.d:  /// Using monthly downloads to penalize stale packages, logarithm to

```

shows no line numbers



---

_Comment by @BurntSushi on 2018-02-13 12:21_

This has been the behavior of ripgrep since the `0.1.0` release. You can see the logic for [determining whether line numbers are enabled here](https://github.com/BurntSushi/ripgrep/blob/ad3f55b0e51cfedfbd4d7897d7a55e4c884fd0e5/src/args.rs#L653-L664).

---

_Closed by @BurntSushi on 2018-02-13 12:21_

---

_Label `invalid` added by @BurntSushi on 2018-02-13 12:21_

---

_Comment by @arclabs561 on 2019-11-21 20:42_

One can achieve slightly similar behavior by doing
```
$(output) | cat -n | rg PAT
```

...And I also see now that one can just use the `rg -n` flag, too.

---

_Comment by @timotheecour on 2019-11-22 00:00_

likewise `--column` will also force show line+column even with piping

---
