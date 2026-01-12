```yaml
number: 20288
title: "E402: Exempt `gi.require_version[s]()` preceding imports"
type: issue
state: open
author: ferdnyc
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-07T20:05:48Z
updated_at: 2025-09-16T09:15:42Z
url: https://github.com/astral-sh/ruff/issues/20288
synced_at: 2026-01-12T15:54:57Z
```

# E402: Exempt `gi.require_version[s]()` preceding imports

---

_@ferdnyc_

### Summary

Code written using [PyGObject](https://pygobject.gnome.org/) that imports versioned GObject-Introspection (`gi`) typelibs requires version-selection calls to precede the actual module imports, e.g.
    
```python
import gi

# Either    
gi.require_version('Gtk', '4.0')
gi.require_versions({'GObject': '2.0', 'Gio': '2.0'})

from gi.repository import GObject, Gio, Gtk
```
    
Because these calls are necessary and unavoidable with PyGObject, a narrow exception to rule E402 would be valuable.

**Note:** I already have a pull request ready to go that addresses this, but I wanted to formalize the need/request with an issue first.

---

_Comment by @ntBre on 2025-09-08 19:45_

Thanks for the issue (and PR)! There have been a couple of recent discussions about similar issues with E402 (https://github.com/astral-sh/ruff/issues/19928, https://github.com/astral-sh/ruff/issues/19904). I think we may want to handle all of these together instead of adding special casing for `gi`.

---

_Label `rule` added by @ntBre on 2025-09-08 19:46_

---

_Label `needs-decision` added by @ntBre on 2025-09-08 19:46_

---

_Comment by @ferdnyc on 2025-09-08 20:00_

@ntBre Having some sort of general configuration for "functions to allow interspersed with imports" would certainly be more flexible. Though it's a slippery slope to the land of ESLint-level configurability. **#ThereBeDragons**

---

_Comment by @MichaReiser on 2025-09-16 07:37_

Allowlisting `PyGObject` by default seems too library-specific to me. 

Are those `gi.require_version` calls required in many modules or just a few? 

---

_Comment by @ferdnyc on 2025-09-16 08:26_

@MichaReiser 

Technically/Officially, anything that uses GObject-Introspection.

Practically, any module that'll be the _first_ one to load a multi-versioned introspection library. (So, any runnable module, as opposed to a pure library module.)

Which is to say, it isn't strictly necessary here:

```python
import gi

gi.require_version("GLib", "2.0")
from gi.repository import GLib
```

...Because GLib only has one version (for the moment). 

But the _only_ way to correctly load either Gtk3 or Gtk4 is:

```python
import gi

gi.require_version("Gtk", "3.0")
from gi.repository import Gtk

# or
gi.require_version("Gtk", "4.0")
from gi.repository import Gtk
```

Otherwise, you end up with this:
```python3
>>> import gi
>>> from gi.repository import Gtk
<python-input-1>:1: PyGIWarning: Gtk was imported without specifying a version first. Use gi.require_version('Gtk', '4.0') before import to ensure that the right version gets loaded.
```
Which technically will work to load Gtk4, but it isn't supported or a good idea to rely on that, because there _will_ eventually be a Gtk5.

...Once you've done the versioned import, you **can** import a _different_ Python module that contains just this:

```python
import gi
from gi.repository import Gtk
```

...and that'll be fine, because the version has already been specified to the introspection loader and that's a global setting. (It isn't actually possible to load e.g. `Gtk-3.0` and `Gtk-4.0` together in the same runtime.)

Also, because skipping the `gi.require_version()` call means making absolutely sure your module can never be loaded by something that _hasn't_ set the version, it's not wrong to, and some developers do, include the `gi.require_version()` call before **every** import of at least a multi-versioned library.

(And some devs do it for all libraries, period, because if `GObject-3.0` or `Gio-3.0` is released tomorrow, everybody's code that imports `GObject` or `Gio` _without_ first setting a version will break just like my `Gtk` example above. They'll get the warning, but even worse the import will default to `GObject`/`Gio` 3.0 unexpectedly, and their code won't be compatible with that.)

I agree it's very domain-specific, but PyGObject is pretty widely used and the calls are unavoidable if you do use it. More to the point, it's not really any more specific than the existing pytest or (especially) matplotlib exceptions.

(And I'm sympathetic (to a point) to an argument like, "OK, but we don't want _even more_ exceptions than we already have." But... only to a point. Because it seems like those exceptions, and this one, demonstrate that there's a need to _either_ keep making exceptions, or to find a solution that avoids the need for **any** exceptions. As I said in the discussion at #20289, I'm _**all for**_ a general configuration-based solution that would make this exception unnecessary.)

---

_Comment by @ferdnyc on 2025-09-16 08:32_

(Regarding the need for these calls in general, _**believe me**_ I have shaken my fist and lamented, at length, that when this was initially being developed someone didn't decide, early on, to go with something like this instead:

```python
import gi
from gi.repository import Gtk4 as Gtk
```

But, that train has sailed.)

---

_Comment by @ferdnyc on 2025-09-16 08:36_

> I agree it's very domain-specific, but PyGObject is pretty widely used

In fact, GitHub code search finds [40,400 Python files containing `gi.require_version`](https://github.com/search?q=%2Fgi.require_version%2F+language%3APython&type=code&l=Python).

(**Edit:** Of those, [120 match `/gi.require_version.*E402/`](https://github.com/search?q=%2Fgi.require_version.*E402%2F+language%3APython&type=code) and [256 match `/gi.require_version.*noqa/`](https://github.com/search?q=%2Fgi.require_version.*noqa%2F+language%3APython&type=code).)

(**Edit2:** Python files matching `/matplotlib\.use/`: [242,000](https://github.com/search?q=%2Fmatplotlib%5C.use%2F+language%3APython&type=code) (😲!!). Python files matching `/pytest\.importorskip/`: [54,800](https://github.com/search?q=%2Fpytest%5C.importorskip%2F+language%3APython&type=code).)

---
