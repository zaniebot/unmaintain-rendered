```yaml
number: 5891
title: Unstable formatting with end-of-line dangling empty dict comment
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
  - help wanted
assignees: []
created_at: 2023-07-19T18:36:18Z
updated_at: 2023-07-24T09:09:51Z
url: https://github.com/astral-sh/ruff/issues/5891
synced_at: 2026-01-12T15:54:45Z
```

# Unstable formatting with end-of-line dangling empty dict comment

---

_@konstin_

The following snippet causes unstable formatting:
```python
a = {  # comment
}
```
We're moving the end-of-line comment to an own line position, where the two leading whitespace for the end-of-line comment are wrong. A solution would we to split the dangling comments and format the end-of-line comment in an end-of-line position.
```
--- Formatted once
+++ Formatted twice
@@ -1,3 +1,3 @@
 a = {
-      # comment
+    # comment
 }
---

Formatted once:
---
a = {
      # comment
}
---

Formatted twice:
---
a = {
    # comment
}
---
```

---

_Label `bug` added by @konstin on 2023-07-19 18:36_

---

_Label `formatter` added by @konstin on 2023-07-19 18:36_

---

_Label `help wanted` added by @konstin on 2023-07-19 18:37_

---

_Comment by @MichaReiser on 2023-07-19 21:08_

It seems like the comment gets formatted as trailing comment (leading spaces). 

[Playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsogCiMRdQAGKQbC1QW5ooPPIDxJx4upAADtIAMwARhcN1E+Fg0AA1ihVhhoLh0ls+HhBGBQBBRDJARh+JwPpkeIRBrp8HBirFRMRCP9-pliMQMOIpLIlBoMRTYFTbpBafTGcz0rhZsTSbh+BpyZSUNSoHBEEgiq9EZpEbhpDKeXK+WMMrMiPBZB0eNJ-uhtcU2ABfXpQ2HwjBkJFoQm6LHYyBLJRoHBodAYHgSf4sziZMoZLXnbFegD0wf+ENjUDjCkUCZDydjkDjAbkceISwkj1jz1EWCWKCwMKD5sD8SwKH+aNwVpQtvt0LhCI4GjIKCUHr5bRSUkIWwosywioxTE7uQdPedXF4-AxntEHG4fAE+LSBumxDODEYC8h3adOBDmSrAh+AmHOcVyDDZWws4wSEJSwwcOkSAZJgHKSEkPBcrK55QEuV4IICygaEsJ6blAKKcE26jaH8BghJwYQAMJwdICFIQAOqRxAmAY5EGPh5FhORlFhCE5FoMABw2gYGCsexNphN42ZQBI-AYHUajfHsuguFBkAwQiAyKBIxDSnQKGQBI0BaCqygLHMSSLCsdDSRAdqLpeCIAI6EAgiQbnquDTKKGBWTZVB-GgCCEFOgnqYQa6ObMLm2e5nnefKYgIFgx46RoznWcFdARV5lA+dAcgIJwmBUDOFq6DwiiEB2Jldo6CInPgT5evqmQYEMEhuTGKaQEYGlwoJohGNAxBumU7VQEYmT-NCTZ9c1dQlGg1Dyh142wGgRgBhSfngaNRh1SgRirQosCFZttxnsVZmlUGmWyJwIYZCtql8i6uDqCU0B8HIsznf8l0YgCShgVNN1IqKwH-JwHo2jJcl1ha2BVjWUqVTS+XoSe3K8jm8QoCGHwSAg4iUHOtyiGagMLKW2LllABW4Hw9UYCg22EI9gYusU2DQkyDWnuF5OU7MNNwHTiSYAGOCKI9GRzgdYCmRex0aO8iQJLD0EdGUD61dA9W4ym-WjTLPBy+U02oTrevExApN9AUtRK62qvqxUoPmfiIbTFghLadgeAzshfIvsqEjLdwMx5QVRUSyVy6vZd7u4M9FMPnZz7TF1bMoaI-yul1fz-Ggo2AkoijIJn0A54gPCwNIAB08QMlQH3-DoBv8oOZAZG2iXEI+DcFMQGRQgXiU8BVDe4JIgJ-LgSYNxpPBDTZ0xkH8TujVPM+l5wZDl4CK+Z7wo08DC-CJIofx7znCDQootewDnSIlJnbYN3AWzhn8j+jUsZ8IHInAoEgH1LHIJsJb22OoCWyHxjzSBStdHMdQEiklmGnTIFMqzFAxPlQq4VEFq3eMoPYNsqDg32JAEkQ0siYKUNg+GeDaYEIJkQo8+s+RYPqlQ2YNDmT52VHQ4+dIUoc2gJwVB90rLhlmFPSsokMjZDxlAAAQknS4WgmwtgfKNRRyjWyjQAGq8xQJcRQ+cj4NwAPIAGV9GGNGgASWMRYqRDdLjR3DHgeqFM7FGJkfySKcIeCVwMVI-a-DBEENHMIrgNVxG-nwFI5o4UNKKFrIgsCKC2boJDpLaCDsOgkEDCBVxV1MR6gPDVdMKger4IgjqGSgjLoK1khkDCSBhahjgCoVB7ZwrRMUBhb4Cw1ABQ6XyXpGh+kzGpkona3x0STC9NeMgYyuqOzeooApSNdQ5mIENV2eAgzCwDrpa8Gkg4YL5EBAYzpZDcLoGkzpDTZjdxWV+H8IlijK2mXQNZ4UZxM2KPeJ6+xPl8i6RhHgCAgxwRmaIGE6QkB3TqBfD4iCzSQqgNC5Ad1ljhkwEi6M7M+RothXMSKcBJFzQGCi82wsVTp0UJgOewtFC4pyDmTI0J-lLPehgM5R9EpEBWSgj4oLmYIFQYwllKARGZDHMsgpzLZks3abEvkOABBbApmLcKpTGgar5OkOYMxmQDi6fsOVUwDWInwIfDlKyMQYBBOFAKhr3ioCSIQjELhbndMDMULBiQKWvOtty6RmtZITngaNY8NK6VrwZVqTuSwsUYBxaNeFx4k1KGRQ3RAM5YCkoDEfQJQzqytg3CDQ6OIsAzgHHUjSWlrykK0ISXFYIZIWn+AiOqMNoFVWKbMdaGsmrFB4AAVQnp4xISgAAi6KI3vFHfhBVu9ByKGnbChdXUfohqHaOgAsljPyZDx3LtXbgPd2ND0hq6tIXAWBTFzrHZe4g16sAABVj0zobtu-4b7jyTsekXBuFJuDwPUc2TRWa8AaB3TXaAMsl6aUnZwfAFVC05iwAqlh78BbVgyCLC+-rUToUw1jFkOHhagvw80Vt0h-RUDNFAwpOZqqzE-gMRAuktjEFrIc767ZqMeSivR-YalmOkaFnhnVOYGRwQGGaMTuGKNi2ozBeOXpQky1FMoKKdty1QEBI5L2CclQYA0hodC8hdFuuDYO-Ko0yDSGCqhr0tbnTCxvSg3QIIABMcTNL3RJIMDEAA2XzWkOQYgAKyhepYkfJkXjKh1yICOkGhhYBjqXCZs6hTTnVmATLtYA1m2hADaSIZWADEYAX0IAQGAP2lYwD8ADBTEA0AwAAF5MQQEqzxlrIMQCVYAAobrACCVrHXGMph6wgKQYESsgCAA)

---

_Comment by @cnpryer on 2023-07-20 01:39_

Is this dict comprehension or just dict?

```
Module(
    ModModule {
        range: 0..18,
        body: [
            Assign(
                StmtAssign {
                    range: 0..18,
                    targets: [
                        Name(
                            ExprName {
                                range: 0..1,
                                id: "a",
                                ctx: Store,
                            },
                        ),
                    ],
                    value: Dict(
                        ExprDict {
                            range: 4..18,
                            keys: [],
                            values: [],
                        },
                    ),
                    type_comment: None,
                },
            ),
        ],
        type_ignores: [],
    },
)
```

---

_Comment by @dhruvmanila on 2023-07-20 03:46_

That's just a plain dictionary. A dictionary comprehension has it's own node [`ExprDictComp`](https://github.com/astral-sh/RustPython-Parser/blob/db04fd415774032e1e2ceb03bcbf5305e0d22c8c/ast/src/generic.rs#L1320-L1327).

---

_Renamed from "Unstable formatting with end-of-line dangling empty dict comprehension comment" to "Unstable formatting with end-of-line dangling empty dict comment" by @konstin on 2023-07-20 06:00_

---

_Comment by @konstin on 2023-07-20 06:02_

oh yes in the empty case it's just a normal dict. fwiw empty parentheses did also work, i didn't check empty list

---

_Comment by @konstin on 2023-07-24 09:09_

Fixed by #5951

---

_Closed by @konstin on 2023-07-24 09:09_

---
