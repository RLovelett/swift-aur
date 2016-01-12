# Swift Language Package (for Arch Linux)

This repository, and PKGBUILD, should allow for building the [Swift
Language](https://swift.org/) from source for use on Arch Linux.

The goal of this project is to combine any necessary patches with a
`PKGBUILD` defintion that leads to a buildable Swift package.  Any patch
that is provided here is also expected to be submitted upstream for
inclusion in the mainline repository.

## How to build

```
$ git clone https://github.com/RLovelett/swift-aur.git
$ cd swift-aur
$ makepkg --syncdeps --rmdeps [--install]
```

* The `[--install]` is meant to indicate that flag is optional. Only
  provide it if you wish to install the resulting package.

## How to install

There are two available options. The first is just combine the build and
install steps by running `makepkg --syncdeps --rmdeps --install`.

The other is to use `pacman` to install the resulting package from the
`makepkg` build (e.g., `pacman -U swiftc-2.2.20160106a.r245.g162becc-1-x86_64.pkg.tar.xz`).
