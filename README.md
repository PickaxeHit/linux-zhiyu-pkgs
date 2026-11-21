# Zhiyu 软件包仓库

供 Lenovo TB378FC 使用。源码、问题反馈和独立启动镜像见 [linux-zhiyu](https://github.com/PickaxeHit/linux-zhiyu)。软件包和 Nix 缓存见本仓库的 [Releases](https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases)。

Debian、Arch Linux ARM 和 Fedora 的 Zhiyu 软件包及索引目前未发布 GPG 签名。下文允许使用未签名包的设置只放在 Zhiyu 的源配置里，官方源继续使用原有的签名验证规则。Nix 缓存使用单独的签名公钥。

## Debian ARM64

用 `sudoedit /etc/apt/sources.list.d/zhiyu.list` 创建或编辑文件，加入这一行。末尾的 `./` 表示平铺仓库。

```text
deb [arch=arm64 trusted=yes] https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/download/repo-apt/ ./
```

`trusted=yes` 只信任这一条未签名的源。更新索引并查看 Zhiyu 包的候选版本。

```sh
sudo apt update
apt-cache policy linux-zhiyu-modules
```

## Arch Linux ARM

用 `sudoedit /etc/pacman.conf`，在文件末尾加入以下段落。如果已有 `[zhiyu]`，直接更新原段落。

```ini
[zhiyu]
SigLevel = Optional TrustedOnly
Server = https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/download/repo-pacman/
```

`Optional TrustedOnly` 允许这个仓库的包和数据库暂时没有签名，同时会验证已有的签名，并要求签名密钥受信任。更新系统和仓库索引后可以列出 Zhiyu 软件包。

```sh
sudo pacman -Syu
pacman -Sl zhiyu
```

## Fedora ARM64

用 `sudoedit /etc/yum.repos.d/zhiyu.repo` 创建或编辑文件。

```ini
[zhiyu]
name=Zhiyu ARM64 packages
baseurl=https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/download/
enabled=1
gpgcheck=0
repo_gpgcheck=0
```

保留 `baseurl` 末尾的 `/releases/download/`。DNF 会自动请求 `repodata/repomd.xml`，正好对应 `repodata` Release 的索引。两个 GPG 设置只关闭本仓库的 RPM 包和元数据签名检查，刷新并列出可用包。

```sh
sudo dnf --repo=zhiyu makecache --refresh
dnf --repo=zhiyu list --available
```

## Nix 和 NixOS

Nix 缓存适用于 `aarch64-linux`。用普通 Nix 安装时，将以下两行加入 `/etc/nix/nix.conf`。如果同名配置已存在，把对应的 URL 和公钥追加到现有值。使用 Nix daemon 的系统在修改后重启该服务。

```ini
extra-substituters = https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/download/nix-cache-index/
extra-trusted-public-keys = zhiyu-nix-cache:AqNN4Xz77Tn02npf11wFSZw1dd7Qe4OBLKmjac1MjEo=
```

NixOS 用户可以在 `/etc/nixos/configuration.nix` 或 Flake 导入的 NixOS 模块中加入以下设置，再按现有方式运行 `nixos-rebuild switch`。

```nix
{
  nix.settings = {
    extra-substituters = [
      "https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases/download/nix-cache-index/"
    ];
    extra-trusted-public-keys = [
      "zhiyu-nix-cache:AqNN4Xz77Tn02npf11wFSZw1dd7Qe4OBLKmjac1MjEo="
    ];
  };
}
```

这些设置会追加 Zhiyu 缓存和对应的公钥，保留官方缓存与默认签名验证。配置后可以构建正式仓的内核包。

```sh
nix build github:PickaxeHit/linux-zhiyu#kernel
```

也可以直接从 [Releases](https://github.com/PickaxeHit/linux-zhiyu-pkgs/releases) 浏览软件包和缓存附件。
