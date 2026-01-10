```yaml
number: 1610
title: Improved hover rendering for parameters
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-22T14:50:02Z
updated_at: 2025-11-23T20:20:19Z
url: https://github.com/astral-sh/ty/issues/1610
synced_at: 2026-01-10T01:58:59Z
```

# Improved hover rendering for parameters

---

_Issue opened by @MichaReiser on 2025-11-22 14:50_

Pylance:

https://github.com/user-attachments/assets/96b44cc1-f27a-4344-b9a0-506153b1d71b


ty

https://github.com/user-attachments/assets/2e24e8bd-d349-4381-a04b-02860f29eb17


The missing padding around parameter documentation makes the documentation much harder to scan than Pylance's version. ty also doesn't provide any special highlighting for the versionchanged section at the bottom

---

_Label `server` added by @MichaReiser on 2025-11-22 14:50_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-22 14:50_

---

_Comment by @gabriel-abn on 2025-11-23 19:15_

I dont know if this might be a bad thing to do while using docstrings or something, but Pylance also renders Markdown formatted snippets

<img width="1002" height="667" alt="Image" src="https://github.com/user-attachments/assets/37a775b9-cc91-49f7-9a61-2c897b03e240" />

it would be nice to have it though 

(if so we can move this suggestion to a separated issue)

---

_Comment by @Gankra on 2025-11-23 19:37_

We by-default handle markdown fine, although we might escape certain things like \_underscoreitalics\_ and codeblocks-by-indent.

<img width="597" height="641" alt="Image" src="https://github.com/user-attachments/assets/f0db2cfc-2a0d-410a-bf92-5212cbff08b3" />

---

_Comment by @Gankra on 2025-11-23 19:46_

Repro is just

```
from logging import basicConfig
                    ^^^^^^^^^^^
```

<img width="450" height="222" alt="Image" src="https://github.com/user-attachments/assets/d4aa59ef-d5d6-493c-a080-0ac66eb0aaf8" />


```text
.. versionchanged:: 3.2
       Added the ``style`` parameter.

.. versionchanged:: 3.3
   Added the ``handlers`` parameter. A ``ValueError`` is now thrown for
   incompatible arguments (e.g. ``handlers`` specified together with
   ``filename``/``filemode``, or ``filename``/``filemode`` specified
   together with ``stream``, or ``handlers`` specified together with
   ``stream``.

.. versionchanged:: 3.8
   Added the ``force`` parameter.

.. versionchanged:: 3.9
   Added the ``encoding`` and ``errors`` parameters.
```

<img width="432" height="255" alt="Image" src="https://github.com/user-attachments/assets/623eb9c3-8b39-4ef3-967b-7f37fabb8643" />

```
A number of optional keyword arguments may be specified, which can alter
the default behaviour.

filename  Specifies that a FileHandler be created, using the specified
          filename, rather than a StreamHandler.
filemode  Specifies the mode to open the file, if filename is specified
          (if filemode is unspecified, it defaults to 'a').
format    Use the specified format string for the handler.
datefmt   Use the specified date/time format.
style     If a format string is specified, use this to specify the
          type of format string (possible values '%', '{', '$', for
          %-formatting, :meth:`str.format` and :class:`string.Template`
          - defaults to '%').
level     Set the root logger level to the specified level.
stream    Use the specified stream to initialize the StreamHandler. Note
          that this argument is incompatible with 'filename' - if both
          are present, 'stream' is ignored.
```

---

_Comment by @Gankra on 2025-11-23 20:00_

The former is clearly A Format With Special Syntax we can absolutely detect and handle.

The latter is so barely a format, it's impressive that they detect and render it non-trivially. I tried a simple heuristic about going from indentation to not but lines like this completely scupper it:

```
format    Use the specified format string for the handler.
datefmt   Use the specified date/time format.
style     If a format string is specified, use this to specify the
```

The only clear heuristic is "a single word followed by >1 spaces" (and then any subsequent indented lines are aligned to the post-spaces).

---

_Comment by @gabriel-abn on 2025-11-23 20:17_

Oh, sorry for the mistake... i just added it because i tested it myself and didn't work at all with a fresh start and nearly no special configurations

<img width="1023" height="705" alt="Image" src="https://github.com/user-attachments/assets/fc00fb57-45ca-4d40-9e56-ec972cd0f63b" />

another test but only with a header line

<img width="819" height="491" alt="Image" src="https://github.com/user-attachments/assets/822b335f-b0c3-4804-873b-e49398b3df51" />

my configs in `settings.json` are the following:

```json
"python.languageServer": "None",
"ty.experimental.autoImport": true,
"ty.interpreter": [
		"${workspaceFolder}/.venv/bin/python"
	],
"ty.experimental.rename": true,
```

is there anything that i need to configure to have the same result than yours?? or am i just missing something in my docstring??

**note**: i use tabs instead of spaces

---

_Comment by @Gankra on 2025-11-23 20:18_

The markdown rendering is bleeding edge, I think it's only available in-tree right now (will be in the next release in a few days).

---
