```yaml
number: 1320
title: Help with back referencing not returning all 
type: issue
state: closed
author: securisec
labels:
  - invalid
assignees: []
created_at: 2019-07-09T13:16:06Z
updated_at: 2019-07-09T13:38:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1320
synced_at: 2026-01-12T16:13:23Z
```

# Help with back referencing not returning all 

---

_@securisec_

#### What version of ripgrep are you using?
```
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

Replace this text with the output of `rg --version`.

#### How did you install ripgrep?

`Brew`

#### What operating system are you using ripgrep on?
`osx`

Replace this text with your operating system and version.

#### Describe your question, feature request, or bug.
[stackoverflow](https://stackoverflow.com/questions/56942623/regex-match-all-occurrences-using-back-reference/) 

##### pasted from SO

I am trying to use back reference to match all occurrences of an imported class being instantiated using `ripgrep` with the `--pcre2` option enabled. 

First I am looking to see if a class is being imported and then back referencing that to look up where it is instantiated. 

- First attempt: Matches the first occurrence of `new ExifInterface(str)`
My regex is: `(import.+(ExifInterface)).+(new\s\2\(.+\))`

- Second attempt: Matches the last occurrence of `new ExifInterface(str)`. My regex is `(import.+(ExifInterface)).+(?:.+?(new\s\2\(.+\)))`

My `ripgrep` command is `rg --pcre2 --multiline-dotall -U "(import.+(ExifInterface)).+(new\s\2\(.+?\))" -r '$3' -o`

**Question**. How can i match all the occrrences of `new ExifInterface(str)`

Bonus question: In some cases, i am getting a `PCRE2: error matching: match limit exceeded` stderr from `rg`, but cant figure out why. The document length is only 161 lines.

[Link to regex101](https://regex101.com/r/q3DiIL/1)

Consider the following data sample:

```
import android.graphics.Point;
import android.media.ExifInterface;
import android.view.WindowManager;
import java.io.IOException;

public class MediaUtils {
    /* renamed from: a */
    public static float m13571a(String str) {
        if (str == null || str.isEmpty()) {
            throw new IllegalArgumentException("getRotationDegreeForImage requires a valid source uri!");
        }
        try {
            int attributeInt = new ExifInterface(str).getAttributeInt("Orientation", 1);
            if (attributeInt == 3) {
                return 180.0f;
new ExifInterface(str).getAttributeInt("Orientation", 1);
            }
            if (attributeInt == 6) {
                return 90.0f;
            }
```

---

_Comment by @BurntSushi on 2019-07-09 13:21_

It looks like you already got a couple answers on SO. Moreover, this is a PCRE2 question, and I personally don't have time to answer them.

> Bonus question: In some cases, i am getting a PCRE2: error matching: match limit exceeded stderr from rg, but cant figure out why. The document length is only 161 lines.

Again, this is a PCRE2 problem, not a ripgrep problem. PCRE2 is a backtracking engine, so even if the document length is small, because of the exponential worst case behavior of backing engines, you can still get errors like this.

---

_Closed by @BurntSushi on 2019-07-09 13:21_

---

_Label `invalid` added by @BurntSushi on 2019-07-09 13:21_

---

_Comment by @securisec on 2019-07-09 13:24_

@BurntSushi Ah perhaps I should revise exactly what I am asking. 

- Is it possible for me to set the global flag in rg?
- Is it possible for me to set the ungreedy flag in rg?

---

_Comment by @BurntSushi on 2019-07-09 13:38_

> Is it possible for me to set the global flag in rg?

I don't know what you're talking about. Please provide a reference to the PCRE2 documentation that describes the "global flag."

> Is it possible for me to set the ungreedy flag in rg?

Use `(?U)` in your pattern.

---
