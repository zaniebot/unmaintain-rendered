```yaml
number: 1861
title: Optionally suppress decompression errors
type: issue
state: closed
author: raxod502
labels: []
assignees: []
created_at: 2021-05-04T03:36:54Z
updated_at: 2021-05-05T23:24:47Z
url: https://github.com/BurntSushi/ripgrep/issues/1861
synced_at: 2026-01-12T16:13:24Z
```

# Optionally suppress decompression errors

---

_@raxod502_

#### Describe your feature request

When I use the `-z, --search-zip` flag on my filesystem, I am typically met with a deluge of error messages like this, typically from the sample files in various open-source repos' test suites:

```
python/distlib/tests/fake_archives/coverage-3.5.2.tar.gz: 
-------------------------------------------------------------------------------
gzip: python/distlib/tests/fake_archives/coverage-3.5.2.tar.gz: unexpected end of file
-------------------------------------------------------------------------------
python/distlib/tests/fake_archives/coverage-3.4b2.tar.gz: 
-------------------------------------------------------------------------------
gzip: python/distlib/tests/fake_archives/coverage-3.4b2.tar.gz: unexpected end of file
-------------------------------------------------------------------------------
python/distlib/tests/fake_archives/coverage-3.3.1.tar.gz: 
-------------------------------------------------------------------------------
gzip: python/distlib/tests/fake_archives/coverage-3.3.1.tar.gz: unexpected end of file
-------------------------------------------------------------------------------
```

I would like to optionally be able to suppress the reporting of decompression errors. One way to implement this feature would be a new long option `--suppress-zip-errors`, or something along those lines. The option could be added to one's `RIPGREP_CONFIG_PATH` file, if one so desires.

The precise behavior would be that if ripgrep cannot decompress a file, then it will silently ignore that file, will not output to stderr, and will exit with a zero return code if no other errors occur.

The usage of ripgrep with this option would be `rg -z --suppress-zip-errors`.


---

_Comment by @BurntSushi on 2021-05-04 11:14_

@raxod502 Have you tried using `--no-messages`?

---

_Comment by @raxod502 on 2021-05-05 23:24_

Ah, thank you, that does exactly what I wanted.

---

_Closed by @raxod502 on 2021-05-05 23:24_

---
