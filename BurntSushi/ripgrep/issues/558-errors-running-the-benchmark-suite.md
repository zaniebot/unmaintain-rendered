```yaml
number: 558
title: Errors running the benchmark suite
type: issue
state: closed
author: svmhdvn
labels: []
assignees: []
created_at: 2017-07-16T18:03:41Z
updated_at: 2017-07-17T13:59:40Z
url: https://github.com/BurntSushi/ripgrep/issues/558
synced_at: 2026-01-12T16:13:22Z
```

# Errors running the benchmark suite

---

_@svmhdvn_

What is the correct usage of the benchmark python script? I tried to run it based on the information posted here: http://blog.burntsushi.net/ripgrep/#benchmark-runner.
Specifically, running `$ ./benchsuite --dir /downloaded/data/dir --list` gives me this:
```
Traceback (most recent call last):
  File "./benchsuite", line 1322, in <module>
    main()
  File "./benchsuite", line 1242, in main
    disabled_cmds=args.disabled.split(','),
AttributeError: 'NoneType' object has no attribute 'split'
```
My python3 version is `Python 3.6.1`

---

_Closed by @BurntSushi on 2017-07-17 12:22_

---

_Comment by @BurntSushi on 2017-07-17 12:22_

There was a bug. Should be all set now. Thanks for reporting!

---

_Comment by @svmhdvn on 2017-07-17 13:14_

I'm not sure if they're fully resolved, but at least I'm able to list the benchmarks available. Now, after downloading "subtitles-ru" and trying to run `$ ./benchsuite --allow-missing --dir /data/dir 'subtitles_ru_literal'`, I'm getting a similar error:
```
Traceback (most recent call last):
  File "./benchsuite", line 1323, in <module>
    main()
  File "./benchsuite", line 1270, in main
    disabled_cmds=args.disabled.split(','),
AttributeError: 'NoneType' object has no attribute 'split'
```

---

_Comment by @BurntSushi on 2017-07-17 13:29_

@sivamahadevan I pushed another commit. Sorry. Try again?

---

_Comment by @svmhdvn on 2017-07-17 13:59_

Yes, it seems to work, thanks for the fixes!

---
