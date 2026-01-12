```yaml
number: 5896
title: lambda arguments have the wrong range and cause unstable formatting
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-07-19T19:26:25Z
updated_at: 2023-07-21T15:39:39Z
url: https://github.com/astral-sh/ruff/issues/5896
synced_at: 2026-01-12T15:54:45Z
```

# lambda arguments have the wrong range and cause unstable formatting

---

_@konstin_

This example causes unstable formatting:
```python
y=lambda:{
#
}
```

```
--- Formatted once
+++ Formatted twice
@@ -1,3 +1,3 @@
 y = (
-    lambda: {}#
+    lambda: {}  #
 )
---

Formatted once:
---
y = (
    lambda: {}#
)
---

Formatted twice:
---
y = (
    lambda: {}  #
)
---
```

The comment gets attached to the lambda arguments instead of the body expression where it should be:
```
{
    Node {
        kind: Arguments,
        range: 2..14,
        source: `lambda:{⏎`,
    }: {
        "leading": [],
        "dangling": [
            SourceComment {
                text: "#",
                position: OwnLine,
                formatted: true,
            },
        ],
        "trailing": [],
    },
}
```

---

_Label `bug` added by @konstin on 2023-07-19 19:26_

---

_Label `formatter` added by @konstin on 2023-07-19 19:26_

---

_Comment by @MichaReiser on 2023-07-19 21:02_

Huh, that's strange. The comment is clearly inside of the dictionary. 

[Playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsogCiMRdQAGKQbC1QW5ooPPIDxJx4upAADtIAMwARhcN1E+Fg0AA1ihVhhoLh0ls+HhBGBQBBRDJARh+JwPpkeIRBrp8HBirFRMRCP9-pliMQMOIpLIlBoMRTYFTbpBafTGcz0rhZsTSbh+BpyZSUNSoHBEEgiq9EZpEbhpDKeXK+WMMrMiPBZB0eNJ-uhtcU2ABfXpQ2HwjBkJFoQm6LHYyBLJRoHBodAYHgSf4sziZMoZLXnbFegD0wf+ENjUDjCkUCZDydjkDjAbkceISwkj1jz1EWCWKCwMKD5sD8SwKH+aNwVpQtvt0LhCI4GjIKCUHr5bRSUkIWwosywioxTE7uQdPedXF4-AxntEHG4fAE+LSBumxDODEYC8h3adOBDmSrAh+AmHOcVyDDZWws4wSEJSwwcOkSAZJgHKSEkPBcrK55QEuV4IICygaEsJ6blAKKcE26jaH8BghJwYQAMJwdICFIQAOqRxAmAY5EGPh5FhORlFhCE5FoMABw2gYGCsexNphN42ZQBI-AYHUajfHsuguFBkAwQiAyKBIxDSnQKGQBI0BaCqygLHMSSLCsdDSRAdqLpeCIAI6EAgiQbnquDTKKGBWTZVB-GgCCEFOgnqYQa6ObMLm2e5nnefKYgIFgx46RoznWcFdARV5lA+dAcgIJwmBUDOFq6DwiiEB2Jldo6CInPgT5evqmQYEMEhuTGKaQEYGlwoJohGNAxBumU7VQEYmT-NCTZ9c1dQlGg1Dyh142wGgRgBhSfngaNRh1SgRirQosCFZttxnsVZmlUGmWyJwIYZCtql8i6uDqCU0B8HIsznf8l0YgCShgVNN1IqKwH-JwHo2jJcl1ha2BVjWUqVTS+XoSe3K8jm8QoCGHwSAg4iUHOtyiGagMLKW2LllABW4Hw9UYCg22EI9gYusU2DQkyDWnuF5OU7MNNwHTiSYAGOCKI9GRzgdYCmRex0aO8iQJLD0EdGUD61dA9W4ym-WjTLPBy+U02oTrevExApN9AUtRK62qvqxUoPmfiIbTFghLadgeAzshfIvsqEjLdwMx5QVRUSyVy6vZd7u4M9FMPnZz7TF1bMoaI-yul1fz-Ggo2AkoijIJn0A54gPCwNIAB08QMlQH3-DoBv8oOZAZG2iXEI+DcFMQGRQgXiU8BVDe4JIgJ-LgSYNxpPBDTZ0xkH8TujVPM+l5wZDl4CK+Z7wo08DC-CJIofx7znCDQootewDnSIlJnbYN3AWzhn8j+jUsZ8IHInAoEgH1LHIJsJb22OoCWyHxjzSBStdHMdQEiklmGnTIFMqzFAxPlQq4VEFq3eMoPYNsqDg32JAEkQ0siYKUNg+GeDaYEIJkQo8+s+RYPqlQ2YNDmT52VHQ4+dIUoc2gJwVB90rLhlmFPSsokMjZDxlAAAQknS4WgmwtgfKNRRyjWyjQAGq8xQJcRQ+cj4NwAPIAGV9GGNGgASWMRYqRDdLjR3DHgeqFM7FGJkfySKcIeCVwMVI-a-DBEENHMIrgNVxG-nwFI5o4UNKKFrIgsCKC2boJDpLaCDsOgkEDCBVxV1MR6gPDVdMKger4IgjqGSgjLoK1khkDCSBhahjgCoVB7ZwrRMUBhb4Cw1ABQ6XyXpGh+kzGpkona3x0STC9NeMgYyuqOzeooApSNdQ5mIENV2eAgzCwDrpa8Gkg4YL5EBAYzpZDcLoGkzpDTZjdxWV+H8IlijK2mXQNZ4UZxM2KPeJ6+xPl8i6RhHgCAgxwRmaIGE6QkB3TqBfD4iCzSQqgNC5Ad1ljhkwEi6M7M+RothXMSKcBJFzQGCi82wsVTp0UJgOewtFC4pyDmTI0J-lLPehgM5R9EpEBWSgj4oLmYIFQYwllKARGZDHMsgpzLZks3abEvkOABBbApmLcKpTGgar5OkOYMxmQDi6fsOVUwDWInwIfDlKyMQYBBOFAKhr3ioCSIQjELhbndMDMULBiQKWvOtty6RmtZITngaNY8NK6VrwZVqTuSwsUYBxaNeFx4k1KGRQ3RAM5YCkoDEfQJQzqytg3CDQ6OIsAzgHHUjSWlrykK0ISXFYIZIWn+AiOqMNoFVWKbMdaGsmrFB4AAVQnp4xISgAAi6KI3vFHfhBVu9ByKGnbChdXUfohqHaOgAsljPyZDx3LtXbgPd2ND0hq6tIXAWBTFzrHZe4g16sAABVj0zobtu-4b7jyTsekXBuFJuDwPUc2TRWa8AaB3TXaAMsl6aUnZwfAFVC05iwAqlh78BbVgyCLC+-rUToUw1jFkOHhagvw80Vt0h-RUDNFAwpOZqqzE-gMRAuktjEFrIc767ZqMeSivR-YalmOkaFnhnVOYGRwQGGaMTuGKNi2ozBeOXpQky1FMoKKdty1QEBI5L2CclQYA0hodC8hdFuuDYO-Ko0yDSGCqhr0tbnTCxvSg3QIIABMcTNL3RJIMDEAA2XzWkOQYgAKyhepYkfJkXjKh1yICOkGhhYBjqXCZs6hTTnVmATLtYA1m2hADaSIZXpAAF5oQSDIGgaANBQAAGIStAA)

---

_Comment by @konstin on 2023-07-20 16:03_

i found this again through a different case:
```python
(lambda:(#
),)
```
The lambda arguments here are empty, yet they span the whole lambda range
```
$ cargo run --bin ruff_dev print-ast scratch.py 
[
    Expr(
        StmtExpr {
            range: 0..14,
            value: Tuple(
                ExprTuple {
                    range: 0..14,
                    elts: [
                        Lambda(
                            ExprLambda {
                                range: 1..12,
                                args: Arguments {
                                    range: 1..12,
                                    posonlyargs: [],
                                    args: [],
                                    vararg: None,
                                    kwonlyargs: [],
                                    kwarg: None,
                                },
                                body: Tuple(
                                    ExprTuple {
                                        range: 8..12,
                                        elts: [],
                                        ctx: Load,
                                    },
                                ),
                            },
                        ),
                    ],
                    ctx: Load,
                },
            ),
        },
    ),
]
```

This causes unstable formatting (handled by default comment placement):

```
---
--- Formatted once
+++ Formatted twice
@@ -1,4 +1,3 @@
 (
-    lambda: ()  #
-    ,
+    lambda: (),  #
 )
---

Formatted once:
---
(
    lambda: ()  #
    ,
)
---

Formatted twice:
---
(
    lambda: (),  #
)
---
```

```
{
    Node {
        kind: Arguments,
        range: 1..12,
        source: `lambda:(#⏎`,
    }: {
        "leading": [],
        "dangling": [
            SourceComment {
                text: "#",
                position: EndOfLine,
                formatted: true,
            },
        ],
        "trailing": [],
    },
}
```

---

_Assigned to @konstin by @konstin on 2023-07-20 16:04_

---

_Renamed from "Formatter: dangling own line comment in lambda body causes unstable formatting" to "lambda arguments have the wrong range and cause unstable formatting" by @konstin on 2023-07-20 16:04_

---

_Comment by @konstin on 2023-07-21 15:39_

Fixed in #5944

---

_Closed by @konstin on 2023-07-21 15:39_

---
