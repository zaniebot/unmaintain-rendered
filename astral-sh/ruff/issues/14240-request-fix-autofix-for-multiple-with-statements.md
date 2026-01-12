```yaml
number: 14240
title: "Request: fix Autofix for multiple-with-statements/SIM117"
type: issue
state: open
author: xvlady
labels: []
assignees: []
created_at: 2024-11-10T06:35:03Z
updated_at: 2024-11-11T10:18:43Z
url: https://github.com/astral-sh/ruff/issues/14240
synced_at: 2026-01-12T15:54:53Z
```

# Request: fix Autofix for multiple-with-statements/SIM117

---

_@xvlady_

Now format ruff-code :
```
with A() as a, B() as b:
      pass
```
This code has problem: B don't readable.
We try other variant:
```
with (
      A() as a, 
      B() as b,
):
      pass
```

this code readable.

Real example:
old code:
```
with sms_mock as mock, self.assertRaises(OtpSkipException):
```
new variant
```
        with (
            sms_mock as mock,
            self.assertRaises(OtpSkipException),
        ):
```
long example:
```
        with (
            self.subTest('no qg_status_mf'),
            patch.object(
                AllureManager,
                'get_test_case_custom_fields',
                return_value=[{'name': 'red', 'customField': {'name': 'qg_status_yota'}}]
            ),
            patch.object(SmsPlugins, 'send', return_value=None) as send_mock,
        ):
```

---

_Comment by @MichaReiser on 2024-11-11 10:18_

Thanks for opening the issue and the detailed description.

We intentionally keep the amount of formatting in autofixes to a minimum because we all have different opinions on formatting and it's impossible to write a fix that satisfies everyone. Instead, we expect users to use an autoformatter (like `ruff format`) or manually format the change to their desire. 

I do think it would be reasonable to format each context expression on its own line if they exceed the configured line length when formatted on a single line.

---
