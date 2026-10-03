# maze-snapshots

Automatic btrfs rollback for everything that is not the kernel. snapper takes
the snapshots and snap-pac hooks them to every pacman transaction; this
package makes sure that configuration actually exists on the installed machine
and adds the rollback tooling:

| Command | What it does |
|---|---|
| `maze-rollback` | Lists the snapshots you can go back to and rolls back to one; refuses snapshots that would not actually take effect. Reboot to apply. |
| `maze-enable-rollback` | Switches root to the btrfs default subvolume, the boot setup rollback needs (done by the installer on new Maze installs). |
| `maze-snapshots-setup` | Creates the snapper configuration on the installed system (run once by `maze-snapshots-setup.service`). |

snapper's hourly timeline is deliberately **not** enabled; turn it on with
`sudo snapper -c root set-config TIMELINE_CREATE=yes` if you want it.

The boot chain itself (kernel, UKI, signatures) is covered by
`maze-secureboot`, not by snapshots.

## Installation

> **Part of Maze Linux.** Every Maze Linux system already has it (pulled in by `maze-meta`). It is built around Maze's own system layout, so installing it on another distribution is not supported.

### From the Maze repository

**On Maze Linux** the repository is already configured:

```bash
sudo pacman -S maze-snapshots
```

**On Arch Linux and Arch-based distributions**, add the repository once:

1. Import and trust the Maze signing key:

   ```bash
   curl -O https://mazerepo.berkkucukk.com.tr/packages/mazelinux.gpg
   gpg --show-keys --with-fingerprint mazelinux.gpg
   sudo pacman-key --add mazelinux.gpg
   sudo pacman-key --lsign-key 7C4D515A6B930CB04794CEF6147C8159B3E2EE5F
   ```

   The fingerprint `gpg` prints must be `7C4D 515A 6B93 0CB0 4794  CEF6 147C 8159 B3E2 EE5F`.

2. Add the repository to the end of `/etc/pacman.conf`:

   ```ini
   [mazelinux]
   SigLevel = Required DatabaseOptional
   Server = https://mazerepo.berkkucukk.com.tr/packages
   ```

3. Sync and install:

   ```bash
   sudo pacman -Syu maze-snapshots
   ```

Optionally install `mazelinux-keyring` as well; it keeps the signing key up to date through pacman.

Remove with `sudo pacman -Rns maze-snapshots`.

### Build from source

```bash
sudo pacman -S --needed base-devel git
git clone https://github.com/berk-kucuk/maze-snapshots.git
cd maze-snapshots
makepkg -si
```

## License

Copyright © 2026 Berk Küçük

Released under the GNU General Public License v3.0 — see [LICENSE](LICENSE).
