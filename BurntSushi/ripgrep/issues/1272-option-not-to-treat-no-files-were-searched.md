```yaml
number: 1272
title: "Option not to treat \"no files were searched\" condition as an error?"
type: issue
state: closed
author: rulatir
labels:
  - question
assignees: []
created_at: 2019-04-26T18:34:39Z
updated_at: 2019-04-26T19:27:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1272
synced_at: 2026-01-12T16:13:23Z
```

# Option not to treat "no files were searched" condition as an error?

---

_@rulatir_

#### What version of ripgrep are you using?

ripgrep 0.9.0
-SIMD -AVX

#### How did you install ripgrep?

Inside a docker image based on `openresty:bionic`:

```
RUN apt-get update && \
    apt-get -y install software-properties-common && \
    add-apt-repository -y --update ppa:x4121/ripgrep && \
    apt-get -y install git ripgrep
```

#### What operating system are you using ripgrep on?

```
root@e62e48b00536:/# uname -a
Linux e62e48b00536 4.18.0-17-generic #18-Ubuntu SMP Wed Mar 13 14:34:40 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```
#### Describe your question, feature request, or bug.

```
root@e62e48b00536:/# rg -l -uuu -m1 -f- /cache/platinum 
^KEY: S:[^\t]*\tH:polanica.d\tD:[^\t]*\tU:[^\t]*\tM:[^\t]*$
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
root@e62e48b00536:/# echo $?
1
root@e62e48b00536:/#
```

Feature request: provide an option that will instruct ripgrep to treat the "no files were searched" condition as a normal occurrence and exit with 0 status.

Rationale: I am using ripgrep to crawl nginx proxy cache files and search by different fields in the cache key for the purpose of selective cache purging. It is entirely possible that the cache directory may contain no files (for example if they all have been expired by the server while nobody was visiting the website), and I don't want to have to parse natural language that `rg` prints on stderr just to be sure that the reason for the 1 exit status is "there is nothing to search at the moment" rather than an actual error.

---

_Comment by @rulatir on 2019-04-26 18:40_

With `--no-messages` the error message is suppressed, but `rg` still exits with status 1 in this case.

---

_Renamed from "Option to not treat "no files were searched" condition as an error?" to "Option not to treat "no files were searched" condition as an error?" by @rulatir on 2019-04-26 18:40_

---

_Comment by @BurntSushi on 2019-04-26 19:27_

I'm going to have to decline. The exit status is correct, where `1` is returned when no matches are found. This is consistent with how grep behaves as well. You should be able to work around this by looking at the output itself instead of just the exit status.

---

_Closed by @BurntSushi on 2019-04-26 19:27_

---

_Label `question` added by @BurntSushi on 2019-04-26 19:27_

---
