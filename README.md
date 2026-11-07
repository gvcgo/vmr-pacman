# vmr pacman repository

Signed pacman repository for [`vmr`](https://github.com/gvcgo/version-manager), served from GitHub Pages.

## Install

```bash
curl -fsSLO https://gvcgo.github.io/vmr-pacman/vmr.gpg
sudo pacman-key --add vmr.gpg
sudo pacman-key --lsign-key C387E9AE5C936248D446868C44E5A0323545E858
```

Append to `/etc/pacman.conf`:

```ini
[vmr]
SigLevel = Required DatabaseOptional
Server = https://gvcgo.github.io/vmr-pacman/$arch
```

Then:

```bash
sudo pacman -Syu vmr
```

pacman only provides `/usr/bin/vmr`; vmr installs the SDK versions themselves under `~/.vmr`, and
`pacman -Rns vmr` leaves them in place.

## Layout

- `x86_64/` - `vmr-<version>-<pkgrel>-x86_64.pkg.tar.zst`, `.sig`, `vmr.db`, `vmr.files`
- `vmr.gpg` - public signing key

Everything above is written by the vmr release workflow
(`.github/workflows/release.yml`, job `pacman`) and signed with the key stored in the
`APT_GPG_PRIVATE_KEY` secret of the version-manager repository. Do not edit it by hand.

## Updating

Each published vmr release appends its package and regenerates the database, so
`pacman -Syu` picks up the new version. Until the version-manager repository has
`TAP_GITHUB_TOKEN` and `APT_GPG_PRIVATE_KEY` configured, build the package from the PKGBUILD in
`vmr/packaging/arch/` instead.
