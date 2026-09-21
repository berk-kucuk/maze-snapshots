# Maintainer: Berk Küçük <dev.berkkucukk@gmail.com>
#
# maze-snapshots — automatic btrfs rollback for everything that is not the
# kernel.
#
# maze-secureboot keeps the BOOT chain recoverable. This package covers the
# other half: a bad library, a half-applied upgrade, a broken maze-* package —
# anything that leaves a running system unusable without touching boot.
#
# It is almost entirely glue. snapper does the snapshots, snap-pac hooks them to
# every pacman transaction; what Maze adds is making sure the configuration
# actually exists on the installed machine, which is the step that never
# happens by itself (a scriptlet running inside the ISO build chroot has no
# btrfs root to configure).
#
# Deliberately NOT enabled: snapper's hourly timeline. Pacman-triggered
# snapshots are the point here; a timeline is a separate decision with a
# different disk profile, and the admin can turn it on with
# `snapper -c root set-config TIMELINE_CREATE=yes`.

pkgname=maze-snapshots
pkgver=1.1.0
pkgrel=4
pkgdesc="Maze Linux automatic btrfs snapshots — every pacman transaction is rollback-able"
arch=('any')
url="https://mazelinux.berkkucukk.com.tr"
license=('GPL3')
depends=(
  'snapper'      # the snapshot manager
  'snap-pac'     # the pacman hooks that snapshot before/after every transaction
  'btrfs-progs'
  'util-linux'   # findmnt
)
optdepends=(
  'btrfs-assistant: GUI for browsing and restoring snapshots'
  'maze-secureboot: maze-enable-rollback verifies the boot chain after rewriting the cmdline'
)
install="${pkgname}.install"
source=()

package() {
  cp -a "${startdir}/maze-snapshots/usr" "${pkgdir}/usr"
  chmod 755 "${pkgdir}/usr/bin/maze-snapshots-setup" \
            "${pkgdir}/usr/bin/maze-enable-rollback" \
            "${pkgdir}/usr/bin/maze-rollback"
  chmod 644 "${pkgdir}"/usr/lib/systemd/system/*.service \
            "${pkgdir}"/usr/lib/systemd/system-preset/*.preset
}
