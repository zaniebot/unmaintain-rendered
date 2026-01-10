```yaml
number: 4016
title: "E501: Does not count tab characters"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2023-04-19T06:21:40Z
updated_at: 2023-05-24T06:37:25Z
url: https://github.com/astral-sh/ruff/issues/4016
synced_at: 2026-01-10T11:09:46Z
```

# E501: Does not count tab characters

---

_Issue opened by @MichaReiser on 2023-04-19 06:21_

The rule E501 (and all other places where we call `str.width()`) does not account for tab characters because `char.width()` returns 0 for control characters. [This playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksufkoirhwFbGQqemZDbmw-CgYlLgaPAAWugAcQ43FpeVCbKIAojHTUABikGyVojxKGig88jXEnHi6kAAO0gDMAIwu86L4sNAA1ihDGNC46Rt8eIJgoBCiMlOGH4nB2mR4hFqunwcGKsVExEIx2OmWIxAw4iksk2PxhsDhC0giORqPR6VwXXBkNw-A00NhKHhUDgiCQRQ2ilemleuGk9PxjMJzQyXSI8FkhFwPGkx3Q-OKbAAvo07o9nhgyG80KDdH9-pB+ko0Dg0OgMDwJMcMZxMmUMnypv99QB6C3HG5OqDOhSKV2Wj1OyDO01yZ3EfoSFZOtb6rD9FBYB7mmVm+JYFDHL64eUoJUq+5PF4cDRkFBKXWE6qFThSQgbChdLAsn5MPO5VWFjVcXj8H560Qcbh8ATAtIijrESYMRht24F9U4S2ZeMCA4CCuBlnIa1lbDNjBIUH9DBPaRIDKYTaSJI8XEM2dQDvqmqKCTEOl0ftQCTQLTs5S9N0SR9IMdAuA+kBPi8ACOhAIIkfZCrgHQUhgsHwVQRxoAghANgG36ED2KFdOhCFYTheFMmICBYJOAEaGhcFkXQ1G4ZQ+EwHICCcJgVBNrKug8IohC5hAyrtvOLzjPgG76sKmQYHUEiYY6nqQEYP5PPhohGNAxDamU2lQEYmTHPc6ZGep+CcCUaDUEyOnWbZRimjChG3lG-wzmJ+Zqi8fBoLINbHBkHmfoSmq4OoJTQHwchdMFoU-CcSg3vZEVvBSl7HJwuqKhBUHJrK2DxomtKyQiQmcIZdB4gSgbxCglo7BICDiJQLYLOsMrlQsMaiMJUo1l0KAKLAhCxWamrFNg9xoip05UYNfDKRgo1wBNiSYKaOCKLFGQtt5YDiXOfkYFsPCJAkFWPpKZRrop0DKZ1nrGZZF1XeUDlQL02yfZ5ED9XECRJJgRC4PdeCPc9FQFZJwKWh0WCgv+2B4E2U5fjA8DbhI7ncJ0gnCaJx2+Z2iWKDsOC4PFQ3fDd2MqMUiGvSlyR6UcxxoJZpxKIoyCc9APOIDwsDSAAdPEKJUMlxw6N9RJlmQGTZixxDrgrBTEBkdwCyxPAyQruCSKcRy4O6Cs-jwZnwR0ZBHIjllWzboucGQ4unC7nO8DzCD3IosuwDzbwlJz2YK3AGw2kckeWf0fsIHINlIMl-RyADx1w2dpwITsk7SOx4WBtZCSQl0xypVK8bM0TIlURXe3KVVthdEpVBFVk+tIux9dKE92zKC38gbe30oCSxE5fYSDf983XRjSJ6L82yY+d1AEJmVkS3QJwzPRbBNpdFbcYYPgGTZF1UAAEJ6SgMxaOmmZrpZ9+P1mlkAGojzMij84olkAHkADKP8-6WQAJIANAefBWMwaY2jwMpKU0D-4K21ombYktf7nz6tvXe7cqyYEyAfBSx9jxn3-kIIGkAfyKCTA3G81cFpCRElnTskoSBmivEgsKvwhRjgUj6FQBloYLTqiTE6UBd6hQZhQ9MB49pWjgEzMRDIqJyK6PsXoahiI5iolojQOjOhrQfuNfY9MqFUUXGQYxekEYhUpneAU+izIoyhkJHeKEGKLh-LXQUgYLw1A1LIVefj1EZHkdrSmB4jz8CKAmLMTj6r6g0eaBAaT3SWMJA8dISAorWQDjsBu0o2iBhycgKKAwbSYGKQ6Ra2TclRUQE2WAp8-amkofUhqCQ9rsi1Eabobs9qKDqTkQMmR7hxQSpaJKGBAmdMgpCAYVAdg8HSVgRAzMp7jJQCQs0FNeFjNjHNGuWTAzU0nG8Q51ChGqAWkc0Q6RBkUnRKWCha8HlQGIuiaA+BEicgOT8DAFwqLfI1NsVASQO4-BcOExQ6ZMDFBnokUp+piiQyivMi+rMiAQlaGgjY7NFCYDtsMvkmt+jVIwLUyyBTJzUqUCUhWzS4BtNgB0jO1D0WJLyhBCQWAmylgZj+P8i5N5aFBHUq4EFZTHBeEpcqRc5ICNbv3F6alig8AAKoW0vpARISgAAiFTLKap1QAYROZZA1ihjV5MtXpdKrMzXHAALJtUIlvPVNq7W4Hde1L1rM9LSAhkA7YOrLLBohgAFTLLak1aDw3HFjZOQ1sUhYKxhNwMur8Mzv2ZXgDQrqZbQC2E7X8hrOD4BkrgwkGzHVN3jttBMGR9oB1RZVWK1VG1tQxC2vaaz21UJldIE0KyC5ryxvJLoicaiIEAhsYgSYfFpRzCO7CtFpSFz4YGadfbdptsOvXfmspKayB2q2wdh0R0oUxoSLcbIfwaGqsPcaXRV7qsDHRSyZBpBkVrYGEVGo9oQ2rroC4AAmKiQGqS1B+AANmg7+LkGgfgAFYkN-kuYkHh6HwI+VyKcJEGg9qmgZk8DM6hJQrXfT1Xoa6xIgEVJEFj9a0RgBTTwGgCxXJgBsckIwp5iAGGkmEbjABIcTFCwCnjAPwMA0lxYIFPV274EnJOKcPMkZA4t+NoAMKeZgYB7gSDIGgaAYBRo3iM8pmobaAC8p46CKegMiUGRhbMDoyAYTzbaIiX1epp-g2EkC6eCwZlA0gjMmbMxZqzUobMqcHY5yLzmSj4HFq52UAnfODp80ljIEQgA) shows the problem: Line number four uses tab indention whereas line 5 uses spaces. Ruff only flags line number 5.

This brings up an interesting question... What length should we use for a tab character: 1? 2? 4? 8... I favour either 1 or 4:

* 1: The closest to pylint that counts the number of characters. 
* 4: To my understanding, the most widely used indention? 



---

_Label `bug` added by @MichaReiser on 2023-04-19 06:21_

---

_Comment by @charliermarsh on 2023-04-19 15:33_

I'd vote to treat tabs as length-1.

---

_Comment by @JonathanPlasse on 2023-04-29 08:49_

What do you mean by length - 1?

---

_Comment by @charliermarsh on 2023-04-29 16:29_

Sorry, I meant "length of 1" or "length equal to 1".

---

_Closed by @MichaReiser on 2023-05-24 06:37_

---
