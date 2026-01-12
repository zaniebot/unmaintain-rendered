```yaml
number: 397
title: "ripgrep doesn't correctly search Haskell project"
type: issue
state: closed
author: bitemyapp
labels:
  - question
assignees: []
created_at: 2017-03-08T01:27:27Z
updated_at: 2017-03-08T15:02:01Z
url: https://github.com/BurntSushi/ripgrep/issues/397
synced_at: 2026-01-12T16:13:21Z
```

# ripgrep doesn't correctly search Haskell project

---

_@bitemyapp_

```
[ callen@athens ~/work/almanac-code/aeson-0.11.2.1 master ✗ ]
$ rg skipSpace
[ callen@athens ~/work/almanac-code/aeson-0.11.2.1 master ✗ ]
$ ag skipSpace
Data/Aeson/Parser/Internal.hs
37:                                         skipSpace, string)
125:  skipSpace
132:    k <- str <* skipSpace <* char ':'
133:    v <- val <* skipSpace
137:      then skipSpace >> loop m
151:  skipSpace
158:      v <- val <* skipSpace
161:        then skipSpace >> loop (v:acc)
177:  skipSpace
193:  skipSpace
370:jsonEOF = json <* skipSpace <* endOfInput
375:jsonEOF' = json' <* skipSpace <* endOfInput
```

Reproducible with `stack unpack aeson-0.11.2.1`, `cd aeson-0.11.2.1`, `rg skipSpace`.

---

_Comment by @BurntSushi on 2017-03-08 11:54_

Hmm, cannot reproduce:

```
$ stack unpack aeson-0.11.2.1
Fetched package index.                                                                                    
Populated index cache.    
aeson-0.11.2.1: download
Unpacked aeson-0.11.2.1 to /tmp/ripgrep-397/aeson-0.11.2.1/
$ cd aeson-0.11.2.1/
$ rg skipSpace
Data/Aeson/Parser/Internal.hs
37:                                         skipSpace, string)
125:  skipSpace
132:    k <- str <* skipSpace <* char ':'
133:    v <- val <* skipSpace
137:      then skipSpace >> loop m
151:  skipSpace
158:      v <- val <* skipSpace
161:        then skipSpace >> loop (v:acc)
177:  skipSpace
193:  skipSpace
370:jsonEOF = json <* skipSpace <* endOfInput
375:jsonEOF' = json' <* skipSpace <* endOfInput
```

Could you run the command again with the `--debug` flag and paste the output here?

---

_Label `question` added by @BurntSushi on 2017-03-08 11:54_

---

_Comment by @bitemyapp on 2017-03-08 15:01_

Ahhh, I think it was the gitignore a level up.

```
$ rg --no-ignore skipSpace
Data/Aeson/Parser/Internal.hs
37:                                         skipSpace, string)
125:  skipSpace
132:    k <- str <* skipSpace <* char ':'
133:    v <- val <* skipSpace
137:      then skipSpace >> loop m
151:  skipSpace
158:      v <- val <* skipSpace
161:        then skipSpace >> loop (v:acc)
177:  skipSpace
193:  skipSpace
370:jsonEOF = json <* skipSpace <* endOfInput
375:jsonEOF' = json' <* skipSpace <* endOfInput
```

Sorry for the trouble. Thank you!

---

_Closed by @bitemyapp on 2017-03-08 15:01_

---
