```yaml
number: 1624
title: Unexpected result when using PCRE2
type: issue
state: closed
author: Bios-Marcel
labels:
  - invalid
assignees: []
created_at: 2020-06-22T10:20:04Z
updated_at: 2020-06-23T07:01:03Z
url: https://github.com/BurntSushi/ripgrep/issues/1624
synced_at: 2026-01-12T16:13:23Z
```

# Unexpected result when using PCRE2

---

_@Bios-Marcel_

I wrote a regex that uses repitition in order to match opening and closing braces including chained function calls.

It looks as follows:

```regex
Services.*?lookup\s*\(\s*(.+?)\s*\)\s*\.\s*((?:.*?\((?:\s*\)\s*|(?:(?:.*?\(.*?\))(?1))\s*\)))+)
```

Here you can see it in action working correctly:
https://regex101.com/r/VwXAWq/1

I get exactly the same matches when running this through `grep` with the `-P` flag to enable PCRE.
However, using the `--pcre2` flag in ripgrep, it doesn't match the last brace in case it is an inlined function call.

E.g. the last matching brace, which is the one before the last brace, will be missing here:

.map( bean -> **Services.lookup( BereichRepository.class ).getAll().findById( bean.getBereichIds().iterator().next()** ) );

Am i missunderstanding something?


---

_Comment by @BurntSushi on 2020-06-22 11:54_

Why did you remove the template when writing this issue? Please don't do that. Please fill out the template. Please include the _exact_ and _complete_ commands that you're running, along with expected output, actual output and the input.

If possible, please also simplify the regex.

---

_Label `question` added by @BurntSushi on 2020-06-22 11:54_

---

_Comment by @Bios-Marcel on 2020-06-22 14:14_

Oh sorry, that wasn't on purpose. I must've clicked the wrong button.

---

_Comment by @BurntSushi on 2020-06-22 14:34_

As far as I can tell, this isn't a problem with ripgrep, but rather seems to be rooted in the fact that you're comparing apples and oranges. Namely, GNU grep uses PCRE while ripgrep uses PCRE2. Your regex101 link claims to be using PCRE through PHP, but doesn't list the version of either. Apparently, newer versions (as of 7.3?) of PHP have upgraded to PCRE2: https://www.php.net/manual/en/pcre.installation.php (Note that PCRE 8.x is PCRE and PCRE 10.x is PCRE2.)

One way to validate this hypothesis is to use tools whose use of PCRE is known. GNU grep is PCRE, ripgrep is PCRE2 and `git grep` is also PCRE2:

![pcre-weirdness](https://user-images.githubusercontent.com/456674/85299809-c1918680-b473-11ea-96b9-adf86916cd92.png)

This screenshot shows that `git grep` and ripgrep behave the same, while GNU grep differs. I am not an expert on PCRE, so I cannot tell you why this difference occurs, but it does at least appear to be specific to the PCRE version. So I don't believe this is an issue with ripgrep specifically.

---

_Closed by @BurntSushi on 2020-06-22 14:34_

---

_Label `question` removed by @BurntSushi on 2020-06-22 14:34_

---

_Label `invalid` added by @BurntSushi on 2020-06-22 14:34_

---

_Comment by @Bios-Marcel on 2020-06-22 14:37_

Oh i see. Thanks a lot :)

---

_Comment by @zherczeg on 2020-06-23 06:15_

This is a behavourial change between pcre and pcre2. Recursions are non atomic (can be backtracked into) to follow perl. You can get the same results if you put the recursion part into an atomic block:

Replace `(?1)` with `(?>(?1))`

With this change the pattern is compatible with both engines.

---

_Comment by @BurntSushi on 2020-06-23 07:01_

@zherczeg Thanks for chiming in! Appreciate it. :)

---
