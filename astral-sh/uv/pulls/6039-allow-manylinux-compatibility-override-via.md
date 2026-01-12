```yaml
number: 6039
title: "allow manylinux compatibility override via `_manylinux` module."
type: pull_request
state: merged
author: ChannyClaus
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: no-manylinux
created_at: 2024-08-12T17:35:19Z
updated_at: 2024-08-21T01:58:10Z
url: https://github.com/astral-sh/uv/pull/6039
synced_at: 2026-01-12T16:07:10Z
```

# allow manylinux compatibility override via `_manylinux` module.

---

_@ChannyClaus_

## Summary
resolves https://github.com/astral-sh/uv/issues/5915, not entirely sure if `manylinux_compatible` should be a separate field in the JSON returned by the interpreter or there's some way to use the existing `platform` for it.

## Test Plan
ran the below
```
rm -rf .venv
target/debug/uv venv
# commenting out the line below triggers the change..
# target/debug/uv pip install no-manylinux
target/debug/uv pip install cryptography --no-cache
```

is there an easy way to add this into the existing snapshot-based test suite? looking around to see if there's a way that doesn't involve something implementation-dependent like mocks.

~update: i think the output does differ between these two, so probably we can use that.~ i lied - that "building..." output seems to be discarded.

---

_Converted to draft by @ChannyClaus on 2024-08-12 17:38_

---

_Marked ready for review by @ChannyClaus on 2024-08-12 20:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-15 03:20_

---

_Label `bug` added by @charliermarsh on 2024-08-15 03:20_

---

_Label `compatibility` added by @charliermarsh on 2024-08-15 03:20_

---

_Comment by @charliermarsh on 2024-08-15 03:20_

Thanks! I can own reviewing this.

---

_Comment by @charliermarsh on 2024-08-16 02:08_

Hmm, looking at the source... I think this might be slightly more complicated. We don't just need to check what manylinux compatibility for the current glibc version. We also need to check compatibility for every _prior_ manylinux version... Notice that in pip, they do:

```python
def platform_tags(archs: Sequence[str]) -> Iterator[str]:
    """Generate manylinux tags compatible to the current platform.

    :param archs: Sequence of compatible architectures.
        The first one shall be the closest to the actual architecture and be the part of
        platform tag after the ``linux_`` prefix, e.g. ``x86_64``.
        The ``linux_`` prefix is assumed as a prerequisite for the current platform to
        be manylinux-compatible.

    :returns: An iterator of compatible manylinux tags.
    """
    if not _have_compatible_abi(sys.executable, archs):
        return
    # Oldest glibc to be supported regardless of architecture is (2, 17).
    too_old_glibc2 = _GLibCVersion(2, 16)
    if set(archs) & {"x86_64", "i686"}:
        # On x86/i686 also oldest glibc to be supported is (2, 5).
        too_old_glibc2 = _GLibCVersion(2, 4)
    current_glibc = _GLibCVersion(*_get_glibc_version())
    glibc_max_list = [current_glibc]
    # We can assume compatibility across glibc major versions.
    # https://sourceware.org/bugzilla/show_bug.cgi?id=24636
    #
    # Build a list of maximum glibc versions so that we can
    # output the canonical list of all glibc from current_glibc
    # down to too_old_glibc2, including all intermediary versions.
    for glibc_major in range(current_glibc.major - 1, 1, -1):
        glibc_minor = _LAST_GLIBC_MINOR[glibc_major]
        glibc_max_list.append(_GLibCVersion(glibc_major, glibc_minor))
    for arch in archs:
        for glibc_max in glibc_max_list:
            if glibc_max.major == too_old_glibc2.major:
                min_minor = too_old_glibc2.minor
            else:
                # For other glibc major versions oldest supported is (x, 0).
                min_minor = -1
            for glibc_minor in range(glibc_max.minor, min_minor, -1):
                glibc_version = _GLibCVersion(glibc_max.major, glibc_minor)
                tag = "manylinux_{}_{}".format(*glibc_version)
                if _is_compatible(arch, glibc_version):
                    yield f"{tag}_{arch}"
                # Handle the legacy manylinux1, manylinux2010, manylinux2014 tags.
                if glibc_version in _LEGACY_MANYLINUX_MAP:
                    legacy_tag = _LEGACY_MANYLINUX_MAP[glibc_version]
                    if _is_compatible(arch, glibc_version):
                        yield f"{legacy_tag}_{arch}"
```

So they're testing `_is_compatible` against all versions.


---

_Comment by @charliermarsh on 2024-08-16 02:09_

Maybe we should just settle for testing the current manylinux version though. Otherwise we'll have to test a _bunch_ of versions and pass that around.

---

_Comment by @charliermarsh on 2024-08-16 02:11_

It seems more common that people use blanket implementations anyway:

```python
def manylinux_compatible(*_, **__):  # PEP 600
    return False
```

---

_Comment by @charliermarsh on 2024-08-16 02:12_

The alternative is that we iterate over all glibc versions up to and including the current, call `_is_compatible`, and then pass back a dictionary.

---

_Comment by @charliermarsh on 2024-08-16 02:12_

What do you think @konstin?

---

_Comment by @ChannyClaus on 2024-08-16 13:27_

> Hmm, looking at the source... I think this might be slightly more complicated. We don't just need to check what manylinux compatibility for the current glibc version. We also need to check compatibility for every _prior_ manylinux version... Notice that in pip, they do:
> 
> ```python
> def platform_tags(archs: Sequence[str]) -> Iterator[str]:
>     """Generate manylinux tags compatible to the current platform.
> 
>     :param archs: Sequence of compatible architectures.
>         The first one shall be the closest to the actual architecture and be the part of
>         platform tag after the ``linux_`` prefix, e.g. ``x86_64``.
>         The ``linux_`` prefix is assumed as a prerequisite for the current platform to
>         be manylinux-compatible.
> 
>     :returns: An iterator of compatible manylinux tags.
>     """
>     if not _have_compatible_abi(sys.executable, archs):
>         return
>     # Oldest glibc to be supported regardless of architecture is (2, 17).
>     too_old_glibc2 = _GLibCVersion(2, 16)
>     if set(archs) & {"x86_64", "i686"}:
>         # On x86/i686 also oldest glibc to be supported is (2, 5).
>         too_old_glibc2 = _GLibCVersion(2, 4)
>     current_glibc = _GLibCVersion(*_get_glibc_version())
>     glibc_max_list = [current_glibc]
>     # We can assume compatibility across glibc major versions.
>     # https://sourceware.org/bugzilla/show_bug.cgi?id=24636
>     #
>     # Build a list of maximum glibc versions so that we can
>     # output the canonical list of all glibc from current_glibc
>     # down to too_old_glibc2, including all intermediary versions.
>     for glibc_major in range(current_glibc.major - 1, 1, -1):
>         glibc_minor = _LAST_GLIBC_MINOR[glibc_major]
>         glibc_max_list.append(_GLibCVersion(glibc_major, glibc_minor))
>     for arch in archs:
>         for glibc_max in glibc_max_list:
>             if glibc_max.major == too_old_glibc2.major:
>                 min_minor = too_old_glibc2.minor
>             else:
>                 # For other glibc major versions oldest supported is (x, 0).
>                 min_minor = -1
>             for glibc_minor in range(glibc_max.minor, min_minor, -1):
>                 glibc_version = _GLibCVersion(glibc_max.major, glibc_minor)
>                 tag = "manylinux_{}_{}".format(*glibc_version)
>                 if _is_compatible(arch, glibc_version):
>                     yield f"{tag}_{arch}"
>                 # Handle the legacy manylinux1, manylinux2010, manylinux2014 tags.
>                 if glibc_version in _LEGACY_MANYLINUX_MAP:
>                     legacy_tag = _LEGACY_MANYLINUX_MAP[glibc_version]
>                     if _is_compatible(arch, glibc_version):
>                         yield f"{legacy_tag}_{arch}"
> ```
> 
> So they're testing `_is_compatible` against all versions.

ah, probably should've looked there as well - i can implement the equivalent logic if y'all decide that to be the way to go 🌵

for what it's worth i do think what you point out in https://github.com/astral-sh/uv/pull/6039#issuecomment-2292605485 is probably true 😅 

---

_Comment by @charliermarsh on 2024-08-20 23:56_

I think we should just document it and move forward as-is.

---

_@charliermarsh approved on 2024-08-21 01:38_

---

_Merged by @charliermarsh on 2024-08-21 01:57_

---

_Closed by @charliermarsh on 2024-08-21 01:57_

---

_Branch deleted on 2024-08-21 01:58_

---
