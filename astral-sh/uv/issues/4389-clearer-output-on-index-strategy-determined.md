```yaml
number: 4389
title: "Clearer output on index strategy, determined source index and indexes being queried for uv pip install <package>"
type: issue
state: open
author: danny-todd-oxb
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2024-06-18T17:05:42Z
updated_at: 2024-06-21T09:53:46Z
url: https://github.com/astral-sh/uv/issues/4389
synced_at: 2026-01-10T05:31:37Z
```

# Clearer output on index strategy, determined source index and indexes being queried for uv pip install <package>

---

_Issue opened by @danny-todd-oxb on 2024-06-18 17:05_

### Platform
```sh
➜ uname -a    
Darwin FA-3620-MBPRO14 23.5.0 Darwin Kernel Version 23.5.0: Wed May  1 20:12:58 PDT 2024; root:xnu-10063.121.3~5/RELEASE_ARM64_T6000 arm64
➜ uv --version
uv 0.2.11 (44041bccd 2024-06-11)
```

### Issue
When running `uv pip install` for a given package (or multiple via -r `requirements.txt` in my case) it would be useful as an end-user for the determined source index for the given package to also be output as well as the indexes being queried.

```sh
➜ cat ~/.config/uv/uv.toml  
[pip]
extra-index-url = ["https://private.index1/simple", "https://private.index2/simple", "https://pypi.python.org/simple"]
```
Initally my index list looked like this ^.

```sh
➜ uv pip install python-geohash
  × No solution found when resolving dependencies:
  ╰─▶ Because only python-geohash==0.8.5 is available and python-geohash==0.8.5 has no wheels are available with a
      matching Python ABI, we can conclude that all versions of python-geohash cannot be used.
      And because you require python-geohash, we can conclude that the requirements are unsatisfiable.
```
No compatible package found since this would just be querying https://private.index1/simple but this wasn't obvious to me based on the output. Having this determined source index for this package output inline would have been helpful here, since I was uninformed of changes to index priority/behaviour in uv vs. pip. 

```sh
➜ uv pip install python-geohash --index-strategy unsafe-best-match
Resolved 1 package in 794ms
Installed 1 package in 4ms
 + python-geohash==0.8.5
```
To replicate something close to pip's behaviour I used `--index-strategy unsafe-best-match`. Having the determined source index as well as the indexes that are being queried (for both `unsafe` index-strategies) would be useful here also. Similar to how `pip install <package>` will output:
```sh
Looking in indexes: https://private.index1/simple, https://private.index2/simple, https://pypi.python.org/simple
Collecting <package>
  Downloading <index>/<package>
```


There was increased confusion around this issue due to being uninformed of the changes within `uv` vs. `pip` regarding priority of indexes and default index strategy. I had expected all indexes to be queried similar to pip's default behaviour but the output above lead me to believe I was experiencing an access/auth/compatibility resolution problem. IMO this would be a sensible addition to make these uv-specific changes clearer to end-users as well as reduce troubleshooting required for instances where package sources could be modified (e.g. overlapping config files, env vars added in pipeline processes etc.).

---

_Label `error messages` added by @zanieb on 2024-06-18 17:46_

---

_Comment by @zanieb on 2024-06-19 00:11_

Thanks for the writeup! I think roughly the idea here would be to add hints to the unsatisfiable resolution when "all" versions of a package cannot be used and there are multiple package indexes.

---

_Label `help wanted` added by @zanieb on 2024-06-19 00:11_

---

_Comment by @danny-todd-oxb on 2024-06-21 09:53_

> Thanks for the writeup! I think roughly the idea here would be to add hints to the unsatisfiable resolution when "all" versions of a package cannot be used and there are multiple package indexes.

That sounds good to me. As an out-of-scope idea, I could see functionality similar to the now-deprecated `pip search` being very useful for package search/query usecases - not sure if there's anything like that on your feature stack.


---
