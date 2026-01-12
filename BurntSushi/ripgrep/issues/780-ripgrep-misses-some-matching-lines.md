```yaml
number: 780
title: ripgrep misses some matching lines
type: issue
state: closed
author: josh-duetto
labels:
  - bug
assignees: []
created_at: 2018-02-08T02:39:35Z
updated_at: 2018-02-08T23:26:51Z
url: https://github.com/BurntSushi/ripgrep/issues/780
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep misses some matching lines

---

_@josh-duetto_

#### What version of ripgrep are you using?

ripgrep 0.7.1
-AVX -SIMD

#### What operating system are you using ripgrep on?

OS X 10.11.6 

#### Describe your question, feature request, or bug.

I found a case where `ripgrep` fails to find some matching lines when it should.  If I tweak the pattern slightly, it will find the lines.  Changing the contents of the files also can bring the missing lines back into the result set.

I was searching a repo of several thousand Java files for an endpoint containing the path "/upsert/rateplans".  I initially used "/upsert/rate" as the pattern.  `rg` found some matches, but not the one I was looking for.  Extending the pattern to "/upsert/ratep" brings in the expected match.  Omitting the leading slash like "upsert/rate" will also yield correct results.

I tried stripping down the files to just the matching lines for a minimal test corpus, but `rg` finds everything in that case.  I can restrict the search to just three files and reproduce the problem, however.

#### If this is a bug, what are the steps to reproduce the behavior?

I copied the three files with expected matches to a new directory and stomped most lines with `sed -e '/upsert/!s/[a-zA-Z]/a/g'` (replace all letters with "a" on lines not containing "upsert").

Here are the scrubbed files:
https://gist.github.com/josh-duetto/065e1b579d72164dc4deb7b54d9279a6

Sorry for all the "aaaaa" spam, but it seems to be somewhat necessary to reproduce the bug.
Also if I change the scrub command to `sed -e '/upsert/!s/./-/g'` then the matches show up again, so there seems to be something more going on than just the byte offset of the match text in the corpus.

Expected matches:
```
[josh@yawp rg2]$ grep -nHrF /upsert/rate .
./one.java:115:            .setServer("http://127.0.0.1:9090/upsert/rates");
./three.java:141:    private static final String UPSERT_RATE_PLANS_ENDPOINT = "/v1/upsert/rateplans";
./two.java:43:    private static final String DARI_RATE_PATH = "/upsert/rates";
```

Ripgrep results (missing "three.java:141"):
```
[josh@yawp rg2]$ rg --debug --no-heading -F /upsert/rate
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        '/',
        'u',
        'p',
        's',
        'e',
        'r',
        't',
        '/',
        'r',
        'a',
        't',
        'e'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(/upsert/rate)], limit_size: 250, limit_class: 10 }
one.java:115:            .setServer("http://127.0.0.1:9090/upsert/rates");
two.java:43:    private static final String DARI_RATE_PATH = "/upsert/rates";
```

Tweaked successful match:
```
[josh@yawp rg2]$ rg --debug --no-heading -F upsert/rate
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'u',
        'p',
        's',
        'e',
        'r',
        't',
        '/',
        'r',
        'a',
        't',
        'e'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(upsert/rate)], limit_size: 250, limit_class: 10 }
one.java:115:            .setServer("http://127.0.0.1:9090/upsert/rates");
three.java:141:    private static final String UPSERT_RATE_PLANS_ENDPOINT = "/v1/upsert/rateplans";
two.java:43:    private static final String DARI_RATE_PATH = "/upsert/rates";
```


---

_Comment by @BurntSushi on 2018-02-08 12:38_

Thanks for the awesome bug report! I can indeed reproduce it. It looks like this is a bug in the new Boyer-Moore optimization introduced in the regex library. The heuristic for using Boyer-Moore is a bit complex, which explains why reproducing the bug is so fiddly.

Incidentally, this shares the same root cause as #781 (Boyer-Moore), although it isn't clear if the implementation has two distinct bugs or not, so I will leave this open.

This should be fixed in the next release.

---

_Label `bug` added by @BurntSushi on 2018-02-08 12:38_

---

_Comment by @josh-duetto on 2018-02-08 22:49_

Happy to help!  Thanks for such a great tool!

---

_Closed by @BurntSushi on 2018-02-08 23:26_

---
