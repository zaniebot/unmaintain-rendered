---
number: 5894
title: "Formatter: lambda star argument gets removed"
type: issue
state: closed
author: konstin
labels:
  - bug
  - good first issue
  - formatter
  - help wanted
assignees: []
created_at: 2023-07-19T19:18:49Z
updated_at: 2023-08-02T19:31:23Z
url: https://github.com/astral-sh/ruff/issues/5894
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: lambda star argument gets removed

---

_Issue opened by @konstin on 2023-07-19 19:18_

`lambda *a: ()` gets formatted as `lambda: ()` the varargs argument gets lost

---

_Label `bug` added by @konstin on 2023-07-19 19:18_

---

_Label `formatter` added by @konstin on 2023-07-19 19:18_

---

_Comment by @MichaReiser on 2023-07-19 21:04_

[Playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsogCiMRdQAGKQbC1QW5ooPPIDxJx4upAADtIAMwARhcN1E+Fg0AA1ihVhhoLh0ls+HhBGBQBBRDJARh+JwPpkeIRBrp8HBirFRMRCP9-pliMQMOIpLIlBoMRTYFTbpBafTGcz0rhZsTSbh+BpyZSUNSoHBEEgiq9EZpEbhpDKeXK+WMMrMiPBZB0eNJ-uhtcU2ABfXpQ2HwjBkJFoQm6LHYyBLJRoHBodAYHgSf4sziZMoZLXnbFegD0wf+ENjUDjCkUCZDydjkDjAbkceISwkj1jz1EWCWKCwMKD5sD8SwKH+aNwVpQtvt0LhCI4GjIKCUHr5bRSUkIWwosywioxTE7uQdPedXF4-AxntEHG4fAE+LSBumxDODEYC8h3adOBDmSrAh+AmHOcVyDDZWws4wSEJSwwcOkSAZJgHKSEkPBcrK55QEuV4IICygaEsJ6blAKKcE26jaH8BghJwYQAMJwdICFIQAOqRxAmAY5EGPh5FhORlFhCE5FoMABw2gYGCsexNphN42ZQBI-AYHUajfHsuguFBkAwQiAyKBIxDSnQKGQBI0BaCqygLHMSSLCsdDSRAdqLpeCIAI6EAgiQbnquDTKKGBWTZVB-GgCCEFOgnqYQa6ObMLm2e5nnefKYgIFgx46RoznWcFdARV5lA+dAcgIJwmBUDOFq6DwiiEB2Jldo6CInPgT5evqmQYEMEhuTGKaQEYGlwoJohGNAxBumU7VQEYmT-NCTZ9c1dQlGg1Dyh142wGgRgBhSfngaNRh1SgRirQosCFZttxnsVZmlUGmWyJwIYZCtql8i6uDqCU0B8HIsznf8l0YgCShgVNN1IqKwH-JwHo2jJcl1ha2BVjWUqVTS+XoSe3K8jm8QoCGHwSAg4iUHOtyiGagMLKW2LllABW4Hw9UYCg22EI9gYusU2DQkyDWnuF5OU7MNNwHTiSYAGOCKI9GRzgdYCmRex0aO8iQJLD0EdGUD61dA9W4ym-WjTLPBy+U02oTrevExApN9AUtRK62qvqxUoPmfiIbTFghLadgeAzshfIvsqEjLdwMx5QVRUSyVy6vZd7u4M9FMPnZz7TF1bMoaI-yul1fz-Ggo2AkoijIJn0A54gPCwNIAB08QMlQH3-DoBv8oOZAZG2iXEI+DcFMQGRQgXiU8BVDe4JIgJ-LgSYNxpPBDTZ0xkH8TujVPM+l5wZDl4CK+Z7wo08DC-CJIofx7znCDQootewDnSIlJnbYN3AWzhn8j+jUsZ8IHInAoEgH1LHIJsJb22OoCWyHxjzSBStdHMdQEiklmGnTIFMqzFAxPlQq4VEFq3eMoPYNsqDg32JAEkQ0siYKUNg+GeDaYEIJkQo8+s+RYPqlQ2YNDmT52VHQ4+dIUoc2gJwVB90rLhlmFPSsokMjZDxlAAAQknS4WgmwtgfKNRRyjWyjQAGq8xQJcRQ+cj4NwAPIAGV9GGNGgASWMRYqRDdLjR3DHgeqFM7FGJkfySKcIeCVwMVI-a-DBEENHMIrgNVxG-nwFI5o4UNKKFrIgsCKC2boJDpLaCDsOgkEDCBVxV1MR6gPDVdMKger4IgjqGSgjLoK1khkDCSBhahjgCoVB7ZwrRMUBhb4Cw1ABQ6XyXpGh+kzGpkona3x0STC9NeMgYyuqOzeooApSNdQ5mIENV2eAgzCwDrpa8Gkg4YL5EBAYzpZDcLoGkzpDTZjdxWV+H8IlijK2mXQNZ4UZxM2KPeJ6+xPl8i6RhHgCAgxwRmaIGE6QkB3TqBfD4iCzSQqgNC5Ad1ljhkwEi6M7M+RothXMSKcBJFzQGCi82wsVTp0UJgOewtFC4pyDmTI0J-lLPehgM5R9EpEBWSgj4oLmYIFQYwllKARGZDHMsgpzLZks3abEvkOABBbApmLcKpTGgar5OkOYMxmQDi6fsOVUwDWInwIfDlKyMQYBBOFAKhr3ioCSIQjELhbndMDMULBiQKWvOtty6RmtZITngaNY8NK6VrwZVqTuSwsUYBxaNeFx4k1KGRQ3RAM5YCkoDEfQJQzqytg3CDQ6OIsAzgHHUjSWlrykK0ISXFYIZIWn+AiOqMNoFVWKbMdaGsmrFB4AAVQnp4xISgAAi6KI3vFHfhBVu9ByKGnbChdXUfohqHaOgAsljPyZDx3LtXbgPd2ND0hq6tIXAWBTFzrHZe4g16sAABVj0zobtu-4b7jyTsekXBuFJuDwPUc2TRWa8AaB3TXaAMsl6aUnZwfAFVC05iwAqlh78BbVgyCLC+-rUToUw1jFkOHhagvw80Vt0h-RUDNFAwpOZqqzE-gMRAuktjEFrIc767ZqMeSivR-YalmOkaFnhnVOYGRwQGGaMTuGKNi2ozBeOXpQky1FMoKKdty1QEBI5L2CclQYA0hodC8hdFuuDYO-Ko0yDSGCqhr0tbnTCxvSg3QIIABMcTNL3RJIMDEAA2XzWkOQYgAKyhepYkfJkXjKh1yICOkGhhYBjqXCZs6hTTnVmATLtYA1m2hADaSIZXoQSDIGgaAYBvDQDoAYMIQA)

---

_Referenced in [astral-sh/ruff#6069](../../astral-sh/ruff/issues/6069.md) on 2023-07-25 11:22_

---

_Label `good first issue` added by @MichaReiser on 2023-07-31 13:14_

---

_Label `help wanted` added by @MichaReiser on 2023-07-31 13:14_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 15:49_

---

_Referenced in [astral-sh/ruff#6203](../../astral-sh/ruff/issues/6203.md) on 2023-07-31 16:57_

---

_Comment by @LaBatata101 on 2023-08-01 00:29_

Can I take a shot at this?

---

_Comment by @charliermarsh on 2023-08-01 01:05_

To my knowledge it's unclaimed, go for it. Here's the PR that added initial lambda formatting, for reference: https://github.com/astral-sh/ruff/pull/5806/files#diff-bb97e8c9522dd5f1f97942d8f546fc3c931fc8293e128a1a3360d8dda56c9b7d

---

_Assigned to @LaBatata101 by @MichaReiser on 2023-08-01 05:37_

---

_Referenced in [astral-sh/ruff#6257](../../astral-sh/ruff/pulls/6257.md) on 2023-08-01 21:35_

---

_Closed by @charliermarsh on 2023-08-02 19:31_

---
