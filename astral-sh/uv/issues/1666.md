```yaml
number: 1666
title: Support alternative requirements file encodings
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-02-18T21:03:18Z
updated_at: 2024-03-08T14:58:47Z
url: https://github.com/astral-sh/uv/issues/1666
synced_at: 2026-01-10T05:40:31Z
```

# Support alternative requirements file encodings

---

_Issue opened by @zanieb on 2024-02-18 21:03_

e.g. `UTF16-LE`


> This works, but `poetry export > requirements.txt` creates UTF16-LE file, while `uv pip install -r requirements.txt` expects UTF-8, so I manually need to re-save it in UTF8. It is not a problem, but would be cool if `uv` would detect input file encoding...
> 
> ```
> uv pip install -r .\requirements.txt
> error: failed to read from file `.\requirements.txt`
>   Caused by: stream did not contain valid UTF-8
> ```

_Originally posted by @Warchant in https://github.com/astral-sh/uv/issues/1634#issuecomment-1951402367_
            

---

_Label `enhancement` added by @zanieb on 2024-02-18 21:03_

---

_Label `compatibility` added by @zanieb on 2024-02-18 21:03_

---

_Comment by @mitsuhiko on 2024-02-18 22:46_

For sake of not making an even bigger mess in the ecosystem it would be ideal if this stays as UTF-8. I'm perplexed that poetry would generate a UTF-16 file and I can only assume this is because it picks up some weird default encoding by accident.

---

_Comment by @zanieb on 2024-02-19 00:03_

That's a fair point, it'd be nice to use a consistent encoding.

cc @Secrus

---

_Comment by @dimbleby on 2024-02-19 00:14_

Poetry does not generate a utf-16 file.

If told a filename to write to, it writes utf-8.  However OP is redirecting stdout, presumably the encoding is whatever the python interpreter says it is in that environment 

---

_Comment by @Secrus on 2024-02-19 00:14_

The UTF-16 encoding was most likely picked up from the environment. As I stated in [here](https://github.com/astral-sh/uv/issues/1634#issuecomment-1951496960), stream redirection is not the way to export to requirements file. We use UTF-8 when writing to files.

---

_Comment by @zanieb on 2024-02-19 00:16_

Great thanks for the background! It seems safe to close this. Happy to hear more use-cases if they exist though.

---

_Closed by @zanieb on 2024-02-19 00:16_

---

_Label `wontfix` added by @zanieb on 2024-02-19 00:16_

---

_Closed by @BurntSushi on 2024-03-08 14:10_

---

_Label `wontfix` removed by @zanieb on 2024-03-08 14:58_

---
