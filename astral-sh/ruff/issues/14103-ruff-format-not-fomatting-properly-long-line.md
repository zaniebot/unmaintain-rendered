```yaml
number: 14103
title: "`ruff format` not fomatting properly long line"
type: issue
state: closed
author: imperosol
labels:
  - formatter
assignees: []
created_at: 2024-11-05T10:48:28Z
updated_at: 2024-11-05T12:44:05Z
url: https://github.com/astral-sh/ruff/issues/14103
synced_at: 2026-01-10T11:09:55Z
```

# `ruff format` not fomatting properly long line

---

_Issue opened by @imperosol on 2024-11-05 10:48_

- keywords search : `format`, `E501`
- commands invoked : `ruff format`, `ruff format --isolated`
- current Ruff version : `0.6.9`
- relevant Ruff settings : `line-length: 88`

Hello,

I am currently working on a django project, where I do some chained queryset operations.
Usually, `ruff format` handles quite well this case and performs line breaks as intended.

However, I encountered a case where it seems to fail. It more or less looks like this :

```py
class Bar:
    def bar_function():
        my_not_so_short_subquery: QuerySet[MyCustomModel] = (
            MuCustomModel.objects.custom_qs_method().filter(first_filter__user=OuterRef("pk"), second_filter__lt=threshold)
        )
```
Small note : This is not the exact same code where I saw the issue, so I changed some variables names to have something that still fails while not being the real deal (but this fails as well, so the main goal as a minimum reproducible code is fulfilled)

Here, the length of penultimate line is of 115 characters, while my ruff config has the default (88 characters). This is really annoying, because it is not only wrongly formatted, but it also conflict with the E501 rule.

What I intended ruff format to do was something more like :
```py
        my_not_so_short_subquery: QuerySet[MyCustomModel] = (
            MuCustomModel.objects.custom_qs_method()
            .filter(first_filter__user=OuterRef("pk"), second_filter__lt=threshold)
        )
```
However, if I try to write this manually, ruff format this back to the wrong version.

If I remove the type annotation (which I do not intend to do), it works : 
```py
        my_not_so_short_subquery = MuCustomModel.objects.custom_qs_().filter(
            first_filter__user=OuterRef("pk"), second_filter__lt=threshold
        )
```
And if I shorten the variable name (which would somewhat hinder the meaning of the variable, thus I'd like to avoid), it works too :
```py
        my_subquery: QuerySet[MyCustomModel] = MuCustomModel.objects.custom_qs_method().filter(
            first_filter__user=OuterRef("pk"), second_filter__lt=threshold
        )
```

Do you know where this could originate from ?

Thank you for your work.

---

_Comment by @MichaReiser on 2024-11-05 11:01_

Thanks for creating a minimal reproducible example. This seems to be fixed in the preview style, see [playground](https://play.ruff.rs/dbf61fe6-9e2c-496e-bc31-192f9e601d4a). The fix will get released as part of the #13371 

---

_Label `formatter` added by @MichaReiser on 2024-11-05 11:01_

---

_Comment by @imperosol on 2024-11-05 12:06_

Indeed, this is fixed with the `--preview` flag. I guess we will just add a `fmt: off` until it is stabilized.

Thanks for your answer.

---

_Comment by @MichaReiser on 2024-11-05 12:44_

I'll close this as the bug is fixed and we're only waiting for stabilization now. Thanks again for reporting!

---

_Closed by @MichaReiser on 2024-11-05 12:44_

---
