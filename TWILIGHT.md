# doomgeneric for Twilight OS

This repository contains the Twilight OS port of
[ozkl/doomgeneric](https://github.com/ozkl/doomgeneric), based on upstream
commit `dcb7a8dbc7a16ce3dda29382ac9aae9d77d21284`. Twilight OS pins this fork
as the `userspace/doomgeneric` submodule. Commands below run from the OS
repository root unless specified otherwise.

The upstream license is preserved in `LICENSE`. Generated objects and
executables are intentionally excluded from Git.

## Twilight changes

- `doomgeneric/doomgeneric_twilight.c` implements the `/dev/fb0`,
  `/dev/input/event0`, and `/dev/input/mice` backend.
- `doomgeneric/Makefile.twilight` builds a static `fbdoom` executable.
- `doomgeneric/doomgeneric.h` exposes mouse input to the generic input layer.
- `doomgeneric/i_input.c` posts Twilight keyboard and mouse events.
- `doomgeneric/i_system.c` raises the default and minimum Doom zone memory.

## Build

Build and install all userspace programs, including `fbdoom`:

```bash
make userspace
```

Build only Doom:

```bash
make -C userspace/apps/fbdoom
```

The standalone build produces `userspace/apps/fbdoom/fbdoom`. The normal
userspace builder copies that executable to `rootfs/bin/fbdoom`.

## Run

Build the OS image and launch QEMU:

```bash
make
make run
```

Inside Twilight OS:

```sh
fbdoom -iwad /doom.wad
```

Doom game data is not part of doomgeneric. A compatible IWAD must be present
in the filesystem image.

## Updating upstream

Develop and commit port changes in this repository first, then push them to
`akashKarmakar02/doomgeneric` before updating the OS gitlink. To import an
upstream revision, merge it on the port branch and retain the Twilight files
and changes listed above. Record the new upstream base here, build with
`make -f Makefile.twilight CC=musl-gcc` inside `doomgeneric/`, and verify the
port in Twilight OS before publishing the new pin.

Upstream history and its license remain intact. WADs, generated objects, and
executables do not belong in this repository.
