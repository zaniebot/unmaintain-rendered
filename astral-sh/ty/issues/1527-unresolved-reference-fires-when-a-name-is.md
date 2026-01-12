```yaml
number: 1527
title: "`unresolved-reference` fires when a name is declared but not bound"
type: issue
state: closed
author: winterqt
labels: []
assignees: []
created_at: 2025-11-11T21:06:40Z
updated_at: 2025-11-12T18:21:00Z
url: https://github.com/astral-sh/ty/issues/1527
synced_at: 2026-01-12T15:54:25Z
```

# `unresolved-reference` fires when a name is declared but not bound

---

_@winterqt_

### Summary

In Nixpkgs, we construct a file that looks like this for type checking our test scripts:
```python
# This file contains type hints that can be prepended to Nix test scripts so they can be type
# checked.

from test_driver.debug import DebugAbstract
from test_driver.driver import Driver
from test_driver.vlan import VLan
from test_driver.machine import Machine
from test_driver.logger import AbstractLogger
from typing import Callable, Iterator, ContextManager, Optional, List, Dict, Any, Union
from typing_extensions import Protocol
from pathlib import Path
from unittest import TestCase


class RetryProtocol(Protocol):
    def __call__(self, fn: Callable, timeout: int = 900) -> None:
        raise Exception("This is just type information for the Nix test driver")


class PollingConditionProtocol(Protocol):
    def __call__(
        self,
        fun_: Optional[Callable] = None,
        *,
        seconds_interval: float = 2.0,
        description: Optional[str] = None,
    ) -> Union[Callable[[Callable], ContextManager], ContextManager]:
        raise Exception("This is just type information for the Nix test driver")


class CreateMachineProtocol(Protocol):
    def __call__(
        self,
        start_command: str | dict,
        *,
        name: Optional[str] = None,
        keep_vm_state: bool = False,
    ) -> Machine:
        raise Exception("This is just type information for the Nix test driver")


start_all: Callable[[], None]
subtest: Callable[[str], ContextManager[None]]
retry: RetryProtocol
test_script: Callable[[], None]
machines: List[Machine]
vlans: List[VLan]
driver: Driver
log: AbstractLogger
create_machine: CreateMachineProtocol
run_tests: Callable[[], None]
join_all: Callable[[], None]
serial_stdout_off: Callable[[], None]
serial_stdout_on: Callable[[], None]
polling_condition: PollingConditionProtocol
debug: DebugAbstract
t: TestCase
installer: Machine; target: Machine;
vlan1: VLan;


installer.start()

installer.wait_for_unit("multi-user.target")

with subtest("Assert readiness of login prompt"):
    installer.succeed("echo hello")

with subtest("Wait for hard disks to appear in /dev"):
    installer.succeed("udevadm settle")

installer.succeed(
    "flock /dev/vda parted --script /dev/vda -- mklabel msdos"
    + " mkpart primary linux-swap 1M 1024M"
    + " mkpart primary ext2 1024M -1s",
    "udevadm settle",
    "mkswap /dev/vda1 -L swap",
    "swapon -L swap",
    "mkfs.ext3 -L nixos /dev/vda2",
    "mount LABEL=nixos /mnt",
)


with subtest("Create the NixOS configuration"):
    installer.succeed("nixos-generate-config  --root /mnt")
    installer.succeed("cat /mnt/etc/nixos/hardware-configuration.nix >&2")
    installer.copy_from_host(
        "/nix/store/xd6h8yqhic3ccvjmchl6q51jj7alms53-configuration.nix",
        "/mnt/etc/nixos/configuration.nix",
    )
    installer.copy_from_host("/nix/store/347hvs3r67gw1l8b768j4gc2m1kk0y1c-secret", "/mnt/etc/nixos/secret")





with subtest("Perform the installation"):
    installer.succeed("nixos-install < /dev/null >&2")

with subtest("Do it again to make sure it's idempotent"):
    installer.succeed("nixos-install < /dev/null >&2")

with subtest("Check that we can build things in nixos-enter"):
    installer.succeed(
        """
        nixos-enter -- nix-build --option substitute false -E 'derivation {
            name = "t";
            builder = "/bin/sh";
            args = ["-c" "echo nixos-enter build > $out"];
            system = builtins.currentSystem;
            preferLocalBuild = true;
        }'
        """
    )



with subtest("Shutdown system after installation"):
    installer.succeed("umount -R /mnt")
    installer.succeed("sync")
    installer.shutdown()

# We're actually the same machine, just booting differently this time.
target.state_dir = installer.state_dir

# Now see if we can boot the installation.

target.start()

target.wait_for_unit("multi-user.target")


with subtest("Assert that /boot get mounted"):
    target.wait_for_unit("local-fs.target")
    target.succeed("test -e /boot/grub")

with subtest("Check whether /root has correct permissions"):
    assert "700" in target.succeed("stat -c '%a' /root")

with subtest("Assert swap device got activated"):
    # uncomment once https://bugs.freedesktop.org/show_bug.cgi?id=86930 is resolved
    target.wait_for_unit("swap.target")
    target.succeed("cat /proc/swaps | grep -q /dev")

with subtest("Check that the store is in good shape"):
    target.succeed("nix-store --verify --check-contents >&2")

with subtest("Check whether the channel works"):
    target.succeed("nix-env -iA nixos.procps >&2")
    assert ".nix-profile" in target.succeed("type -tP ps | tee /dev/stderr")

with subtest(
    "Check that the daemon works, and that non-root users can run builds "
    "(this will build a new profile generation through the daemon)"
):
    target.succeed("su alice -l -c 'nix-env -iA nixos.procps' >&2")

with subtest("Configure system with writable Nix store on next boot"):
    # we're not using copy_from_host here because the installer image
    # doesn't know about the host-guest sharing mechanism.
    target.copy_from_host_via_shell(
        "/nix/store/a2a0bplj5mknpv92d71wvihjhhlsb0n1-configuration.nix",
        "/etc/nixos/configuration.nix",
    )

with subtest("Check whether nixos-rebuild works"):
    target.succeed("nixos-rebuild switch >&2")

with subtest("Test nixos-option"):
    kernel_modules = target.succeed("nixos-option boot.initrd.kernelModules")
    assert "virtio_console" in kernel_modules
    assert "list of modules" in kernel_modules
    assert "qemu-guest.nix" in kernel_modules

target.shutdown()

# Check whether a writable store build works

target.start()

target.wait_for_unit("multi-user.target")


# we're not using copy_from_host here because the installer image
# doesn't know about the host-guest sharing mechanism.
target.copy_from_host_via_shell(
    "/nix/store/qxm4wkdk5868nalqiasqm2k3k44gdk41-configuration.nix",
    "/etc/nixos/configuration.nix",
)
target.succeed("nixos-rebuild boot >&2")
target.shutdown()

# And just to be sure, check that the target still boots after "nixos-rebuild switch".

target.start()

target.wait_for_unit("multi-user.target")

target.wait_for_unit("network.target")

# Sanity check, is it the configuration.nix we generated?
hostname = target.succeed("hostname").strip()
assert hostname == "thatworked"

target.shutdown()

# Tests for validating clone configuration entries in grub menu
```

When experimenting with migrating from mypy to ty, I run into the following issue ([playground](https://play.ty.dev/14c4c40c-36c7-43df-a1cc-ab3895d29f8c)):
```
error[unresolved-reference]: Name `installer` used when not defined
  --> testScriptWithTypes.py:62:1
   |
62 | installer.start()
   | ^^^^^^^^^
63 |
64 | installer.wait_for_unit("multi-user.target")
   |
info: rule `unresolved-reference` is enabled by default
```

Perfectly understandable if this is expected behavior, but given the divergence from mypy, figured I'd file anyways.

### Version

ty 0.0.1-alpha.26

---

_Comment by @carljm on 2025-11-11 22:40_

Thanks for the report. Yeah, it looks like we are the only type checker that does this. I think we should probably just trust the declaration in these cases. Or at least make this a separate rule that can be turned off.

---

_Renamed from "`unresolved-reference` fires when only a type definition is present" to "`unresolved-reference` fires when enclosing scope has a declaration but no bindings" by @carljm on 2025-11-11 22:40_

---

_Added to milestone `GA` by @carljm on 2025-11-11 22:40_

---

_Comment by @MichaReiser on 2025-11-12 07:24_

> Thanks for the report. Yeah, it looks like we are the only type checker that does this. I think we should probably just trust the declaration in these cases. Or at least make this a separate rule that can be turned off.

I'd expect this to trigger a `possibly-unresolved-attribute` or `unresolved-attribute` error instead as the variable is clearly defined, it's just that it is never bound. I believe TypeScript outright disallowes `a: int` in strict mode without an assignment (or when it can't prove that the variable is assigned in all code paths). Instead, you'd have to write it as `a: int | None` if the variable is only sometimes initialized. Which makes sense to me and has prevented many bugs in code that I've written.

Note, that ty doesn't raise an error if the file is a `pyi` file (which, obviously, doesn't work for this use case but seems more correct to me)

https://play.ty.dev/2106b7b7-f822-490f-9da8-4ef2fbe9605f

---

_Comment by @sharkdp on 2025-11-12 08:07_

> it looks like we are the only type checker that does this

Are we? Pyright [reports `"s" is unbound (reportUnboundVariable)`](https://pyright-play.net/?code=M4LgBMAuBOBQAO0CWA7SAKYBKWQ) and pyrefly reports [`s` is uninitialized [unbound-name]](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSIciABHAC4BOAOusQxOnQBRwCULIADQgArnWhwS5RCADE1AKrioEOqWpgR6AMbjc6OCxaYYYDbgYBbVHQD66EZewwGXfDQ51e1ALQA%2BWkZEFmpQ6gYYOhEGdA0mEAA5R2cGGmB8AF94gWEyCLAoUkI6XEsoCnkABVJ8wtoMHAJqbX1IAHNomwh9QhZ5AGUYGGoACzo6YioAeim800LCCzapmHQpzFxtOCmW9HbOvTXzBmpUADdUaFRsWGbWiA6GLv1qXGJDyRYyOhH9HzOXHBurEALzUeIAZkIAEYAEzZdAgDLCVC6CAAgBi0BgFDQWDwRDISKAA). Or which specific code were you testing?

The code is clearly wrong (the variables are declared, but not bound) so I don't see why we should change anything here? It seems reasonable to me that if you want to convince the type checker that something exists which clearly doesn't, that you would have to go out of your way to do so, like maybe using `installer: Machine = cast(Machine, ...)` or similar.

> I'd expect this to trigger a `possibly-unresolved-attribute` or `unresolved-attribute` error instead

Why? Both `s` in the toy example as well as `installer` in the original example are symbols that have been declared, but not bound. So IMO, even just accessing that object should trigger the error, not the attribute access that comes later. I agree that we should maybe split the rule, though. To distinguish between `unresolved-reference` (not declared, not bound) and `unbound-reference` (declared, but never bound). This would provide an alternative solution here (ignore `unbound-reference` across the whole file).

> Instead, you'd have to write it as `a: int | None` if the variable is only sometimes initialized. Which makes sense to me and has prevented many bugs in code that I've written.

That would also be possible in Python, you would just have to set it to `None` explicitly. And then you would also [get `unresolved-attribute` errors](https://play.ty.dev/78b0e6ee-8b82-47aa-be16-c3128c6d8688).

---

_Comment by @carljm on 2025-11-12 15:12_

I should have been clearer in my comment; I was making a distinction in my mind between two different scenarios, which I didn't clarify (except subtly in my edit to the issue title). And I didn't look carefully enough at the OP code sample to realize that it contains examples of both scenarios. 

Other type checkers distinguish between same-scope use of a name and lazy-nested-scope use of a name. The case I tested is

```py
s: str
def f():
    reveal_type(s)
```

Where all three of pyright, mypy, and pyrefly reveal `str` with no error.

I don't think we should change anything in the same-scope scenario that you tested. That's code running eagerly in sequence; using an uninitialized value should be an error.

I think there's a stronger argument for not erroring in a lookup from a lazy scope. In this case, the annotation may be a declaration that "I will set this name in this scope before the function is called, via some external source the type checker cannot see", and I think it's reasonable to trust that declaration, and the fact that all existing type-checkers do so is strong signal.

But since the OP code sample contains uses of the non-initialized names both in module scope and in nested functions, making the change I was suggesting would still not allow this script to type-check. I agree that for this script, the `cast` trick you suggest seems like the best option.

---

_Comment by @carljm on 2025-11-12 18:20_

Uh, and somehow in all of that I _also_ managed to miss the fact that we [already](https://play.ty.dev/4300c635-1708-49fb-8ca2-5fd03e76f273) match mypy/pyright/pyrefly in not erroring on declared-only names in enclosing scopes. Sorry everyone for the unnecessary noise.

I think this should just be closed as "not planned". Thanks for the report, but we don't intend to match mypy's laxness in this scenario.

---

_Closed by @carljm on 2025-11-12 18:20_

---

_Renamed from "`unresolved-reference` fires when enclosing scope has a declaration but no bindings" to "`unresolved-reference` fires when a name is declared but not bound" by @carljm on 2025-11-12 18:21_

---
