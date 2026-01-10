```yaml
number: 6197
title: "Formatter: Pragma comments"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-07-31T13:20:49Z
updated_at: 2023-09-01T11:27:38Z
url: https://github.com/astral-sh/ruff/issues/6197
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: Pragma comments

---

_Issue opened by @MichaReiser on 2023-07-31 13:20_

See https://github.com/astral-sh/ruff/discussions/6670 for the proposal for the details:

* [x] https://github.com/astral-sh/ruff/issues/5630
* [x] #6771
* [x] #6772



---

_Label `formatter` added by @MichaReiser on 2023-07-31 13:24_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 15:48_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-07-31 16:18_

---

_Comment by @davidszotten on 2023-08-04 08:24_

do you have any examples of this happening? (how did you find them)

---

_Comment by @MichaReiser on 2023-08-04 08:36_

> do you have any examples of this happening? (how did you find them)

I haven't looked closely into it but there's some related Black logic:

https://github.com/psf/black/blob/2fd9d8b339e1e2e1b93956c6d68b2b358b3fc29d/src/black/lines.py#L227-L294

and 

https://github.com/psf/black/blob/268dcb677ce80331e0ef104d8335c2ada32872fb/src/black/comments.py#L324-L335

I don't know if this is a valid noqa placement, but splitting by the arguments could change what it annotates

```python
def test(a, b, # noqa: RUFXXX
	c, d): pass

# Formatted
def test(
	a, 
	b, # noqa: RUFXXX
	c, 
	d
): pass
```

This could cause `RUFXXX` to trigger now if it suppressed `a` or `test` and not `b`. [Playground](https://play.ruff.rs/?secondary=Format#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsovYASgDiMRdQAGKQbC1QW5ooPPIDxJx4ukgAAdpABmACMLnuonwsGgAGsUKsMNBcOktnw8IIwKAIKIZCCMPxON9MjxCINdPg4MVYqJiIQgUDMsRiBhxFJZEoNNjqbBaQ9IAymSy2elcLMyRTcPwNFSaSg6VA4IgkEUPijNCjcNJ5fzFYKxhlZkR4LIOjxpED0HrimwAL69WEIpEYMiotAk3S4vGQJZKNA4NDoDA8CRA9mcTJlDK685430AejDQOhCagiYUimT4bTCcgieDckTxCWEheCbeoiwSxQWHhoatIfiWBQQMxuFtKAdTrhiORHA0ZBQSm9graKSkhC2FFmWBV2KYPdyzv7bq4vH42J9og43D4AiJaWN02IZwYjGXML7rpw4cytYE-wEY-zKuQkbK2AXGCQJKWGCItISAZJg3KSEkPC8gqV5QKut4ICCygaEs547lA6KcK26jaICBghJwYQAMKIdIyGoQAOhRxAmAYVEGERVFhFRNFhCEVFoMABz2gYGAcVx9phN4eZQBI-AYHUah-HsuguLBkDwciAyKBIxBynQ6GQBI0BaOqygLHMSSLCsdByRAjorjeyIAI6EAgiTboauDTBKGC2fZVCAmgCCELOIlaYQm4ubM7kOV5Pl+UqYgIFgZ76Robl2WFdDRb5lD+dAcgIJwmBUPO1q6DwiiEN25m9i6yInPgr6+kamQYEMEiefG6aQEY2mIiJohGNAxCemUXVQEYmRAnCraDW1dQlGg1BKt1U2wGgRjBtSgVQRNRiNSgRgbQosAlTtDyXmVlkVaGOWyJw4YZOtGmCu6uDqCU0B8HIsxXUCN3YsCSiQbN92ohKYFApw3r2vJimNta2C1vWso1fSRVYeefICvm8QoOG3wSAg4iUIuDyiJaIMLBWeJVlAxW4HwTUYCge2EC9IbusU2BwqyzUXlFVM07M9NwIziSYMGOCKC9GSLsdYAWdeZ0aF8iQJAjcEdGUz4NdATUE+mQ0TfLPCK+Uc0YfrhtkxAFN9AUtSqx2GtaxUENWUS4bTFgJJ6dgeDzmhgrvmqEhrdwMyFcVpXS+Va4fTdXu4G91PPo5b7TL1nPoaIQIer1gJAmgE0gkoijIDn0D54gPCwNIAB08TMlQ31AjoxtCiOZAZJ2KXEC+zcFMQGSwsXKU8NVze4JIIKArgqbN9pPCjfZ0xkICrsTbP88V5wZBVyC6857wE08PC-CJIogKH-nCBwooDewPnqIlDnnbN3AWxRoCL8TUsl8IHInAoEg30lhyHNtLJ2Z0QQOW+GeaQ6U7r5jqAkCksxM6ZGprWYo2IiolSiigzWXxlB7HtlQKG+xIDklGlkHBSg8FI0IQzYhxNSGniNoKXBTVaGzHoWyIuapGFn0ZOlbm0BOAYKerZKMsxZ41gkhkbIhMoAACFU4AFEtCtnbM+Caqj1EdgmgANQFigZRigi6n2bgAeQAMrGNMRNAAkuYmxsjm7KLjlGPATVqZOLMfIoUMVEQ8BriY2RR0hEiOIROMRXB6pSIAvgWRzQoraUUA2FBkF0GcyweHGWcFnYdBICGcCnjbo4kNMeeqWYVD9SIdBfU8kRE3WVgpDI2EkBiwjHAFQGCuxRXiYobCfwFhqGCj0wUgyNDDJmHTNR+0-hYkmL6O8ZApm9Rdp9RQJTUYGnzMQUaHs8ChjFsHAyd5tKh2wYKUCAw3SyD4XQLJvSWmzD7hs38-5xLFDVvMugWyorzlZsUJ8r19i-MFH07CPAEChkQgs0Q8J0hIEenUa+3wUGWlhVAeFyBHrLCjJgNFcYuaCixYiuYMU4AyMWgMDFVsxbqizooTAi8xaKEJTkfMmQ4TArWV9DAVzT4pSIBs9B3xIVswQBglhHKUDiMyJOdZJT2WLPZt0xJgocACC2NTSWUVKmNB1YKdIcwZhsmHH0-YSqpgmpRPgE+PKNnYgwOCKKwVTVfFQEkEh2IXCPP6SGYouDEg0s+XbflcidYKWnEgiaZ4GVMs3iy3UPclh4owASiayKzxpqUOi5uiB5ywEpcGU+oSxl1g7NucGJ18RYHnMOJp2ldJ3goVoEkhLITyWtECZEjV4ZwNquU2YW1tatWKDwAAqtPXxiQlAABFsUxq+JOoiKqD4jkUPOxFK7er-QjWOydABZXGgVKHTvXZu3AR68anojb1aQuAsCWKXVO29xB71YAACrnoXc3fdQIv1nlnS9UuzdqTcCQdotsui814A0Ae+u0B5arx0rOzg+BqqlvzFgFV7Cv7CzrBkcW19g0Yiwrh3G7ICNi0hcR5onbpBBioJaWBpT8x1VmD-AYiADJbGIA2U5f0uz0e8rFZj+xNLsco6LIjBr8zMkQgMS0UnCM0clvR+CSdfSRPlhKZQsVHbVqgCCFyvtk6qgwNpDQWF5CGK9eG0dRUJpkGkGFTDvpG1ujFg+9BuhwQACYkk6SeuSQY2IABsgXdLcmxAAVki-SxIxTYtmQjrkEEjINBi2DE0xEbZ1AWiurMYmfawBbIdCAe0kQqsrTAJAgw0BmBgDII1gAxGAdI1loB0GuDcFwLhwRgBAAASCwI1tAYQ6CZ1ZEAA)

---

_Comment by @roshanjrajan-zip on 2023-08-06 23:18_

Hi! This is one of the problems we have in our repo. However, we use # pyright: ignore instead of # type: ignore. It would be great to have this as dynamic or something to support different use cases. Please lmk if there is anything I can do to provide more info or any help - https://github.com/psf/black/pull/3661

---

_Comment by @MichaReiser on 2023-08-16 13:49_

Relevant lines in the Black formatter (I think) 

https://github.com/psf/black/blob/2fd9d8b339e1e2e1b93956c6d68b2b358b3fc29d/src/black/lines.py#L227-L294

---

_Comment by @charliermarsh on 2023-08-16 13:52_

My one-second reaction is that I think we probably do a reasonably good job of avoiding the uncollapsible comment case since we don't really collapse comments (comments always expand the parent), but I don't think we handle the unsplittable line case at all.

---

_Comment by @MichaReiser on 2023-08-16 14:09_

Some more relevant issues:
* https://github.com/psf/black/issues/195
* https://github.com/psf/black/issues/1061

---

_Comment by @MichaReiser on 2023-08-17 19:05_

@roshanjrajan-zip The example you brought up on the Black issue is 

```python
some_long_variable_name = some_long_dict_name[0] # pyright: ignore[reportGeneralTypeIssues]
```

How would you preferred formatting look like? 

Would the following work for you?

```python
some_long_variable_name = ( # pyright: ignore[reportGeneralTypeIssues]
	some_long_dict_name[0] 
)
# Or
some_long_variable_name = some_long_dict_name[  # pyright: ignore[reportGeneralTypeIssues]
	0
] 
```

It would still require manual intervention (you have to move the comment to the opening parentheses), but it would allow you to suppress the issue. 


---

_Comment by @MichaReiser on 2023-08-17 19:31_

Black issue and PR that added the special `type: ignore` handling

* https://github.com/psf/black/issues/997
* https://github.com/psf/black/pull/1040

---

_Comment by @MichaReiser on 2023-08-18 07:40_

See https://github.com/astral-sh/ruff/discussions/6670

---

_Comment by @roshanjrajan-zip on 2023-08-19 04:40_

Hi @MichaReiser,

Definitely the first one is much better imo and my team agrees! Thanks so much for your work on the proposal. Taking a look at it!
```python
some_long_variable_name = ( # pyright: ignore[reportGeneralTypeIssues]
	some_long_dict_name[0] 
)
```

---

_Removed from milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:32_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:32_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:32_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:32_

---

_Removed from milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 15:10_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 15:10_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 15:10_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 15:10_

---

_Closed by @MichaReiser on 2023-09-01 11:27_

---
