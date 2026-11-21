# Zhiyu package repositories

Packages for Lenovo TB378FC. Source code, issues, and standalone boot images are hosted in [linux-zhiyu](https://github.com/PickaxeHit/linux-zhiyu).

| System | Repository |
| --- | --- |
| Debian ARM64 | Use the [`repo-apt` Release](https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/tag/repo-apt) download URL as the APT URI, with suite `./`. |
| Arch Linux ARM | Use the [`repo-pacman` Release](https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/tag/repo-pacman) download URL as the `Server` under `[zhiyu]`. |
| Fedora | Use `https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/download/` as the DNF `baseurl`. The [`repodata` Release](https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/tag/repodata) contains both the index and the RPMs. |
| Nix/NixOS | Use `https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/download/nix-cache-index/` as the substituter. |

Add the following public key to Nix's `trusted-public-keys`:

```text
zhiyu-nix-cache:AqNN4Xz77Tn02npf11wFSZw1dd7Qe4OBLKmjac1MjEo=```

See [Releases](https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases) for package and cache assets.
